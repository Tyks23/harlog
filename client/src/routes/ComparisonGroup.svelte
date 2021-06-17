<script>
    import "charts.css";
    import { group_outros } from "svelte/internal";
    import { writable } from "svelte/store";
    import { push } from "svelte-spa-router";
    if(sessionStorage.getItem("token") == null) {
           
           // Redirect to login page
           push("/");
       }
    function logout(){
        sessionStorage.removeItem("token");
        push("/");
    }
    let participants = {};

    let activity1 = false;
    let participant1 = false;

    let activities = {};

    let store = writable(0);

    let data;

    let selected1 = {
        group_id: null,
        activity_id: null,
        part_id: null,
    };

    const graphs = [];

    async function listGroup() {
        const response = await fetch("http://localhost:3000/listgroup", {
            method: "GET", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                Authorization: sessionStorage.getItem("token"),
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
        });

        if (response.ok) {
            return await response.json();
            //push("/");
        }
    }

    async function listActivities(input) {
        const response = await fetch("http://localhost:3000/listactivities", {
            method: "POST", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(input),
        });
        if (response.ok) {
            return await response.json();
            //push("/");
        }
    }

    async function listParticipants(input) {
        const response = await fetch("http://localhost:3000/listparticipants", {
            method: "POST", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(input),
        });

        if (response.ok) {
            return await response.json();
            //push("/");
        }
    }

    let activityEmotions = {
        emotion1: 0,
        emotion2: 0,
        emotion3: 0,
    };
    const getAvgOfCols = (
        array,
        start = 0,
        stop = 0,
        isReversed = undefined
    ) => {
        const newArray = array.slice(start, stop + 1);
        if (isReversed != undefined)
            newArray.forEach((x, i) => {
                if (isReversed[i % isReversed.length]) {
                    newArray[i] = 6 - x;
                }
            });

        return newArray.reduce((acc, cur) => acc + cur, 0) / newArray.length;
    };

    async function getComparisonData(input) {
        const parsed = Object.entries(input).reduce((acc, [key, value]) => {
            if (value !== null) return { ...acc, [key]: value };
            return acc;
        }, {});

        const response = await fetch(
            "http://localhost:3000/getcomparisondata",
            {
                method: "POST", // *GET, POST, PUT, DELETE, etc.
                mode: "cors", // no-cors, *cors, same-origin
                cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
                credentials: "same-origin", // include, *same-origin, omit
                headers: {
                    "Content-Type": "application/json",
                    // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: "follow", // manual, *follow, error
                referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(parsed),
            }
        );
        if (response.ok) {
            data = (await response.json()).rows;
            console.log(data);
            for (let i of data) {
                activityEmotions.emotion1 = i.emotion1 = getAvgOfCols(
                    i.answer,
                    0,
                    1,
                    [false, true]
                );
                activityEmotions.emotion2 = i.emotion2 = getAvgOfCols(
                    i.answer,
                    2,
                    3,
                    [false, true]
                );
                activityEmotions.emotion3 = i.emotion3 = getAvgOfCols(
                    i.answer,
                    4,
                    5
                );
            }
                let chartList = document.getElementById('charts');
                let appendable = document.createElement('tr'); 
                
                for (let i = 0; i < 3; i++) { 
                    let appendable2 = document.createElement('td');
                    let numb = activityEmotions[`emotion${i+1}`];
                    appendable2.setAttribute('style', `--size:${numb}`);
                    appendable2.textContent = activityEmotions[`emotion${i+1}`];    
                    appendable.appendChild(appendable2);
                    console.log(appendable2);
                    
                }
                           
                chartList.appendChild(appendable);
                   
        }
    }
</script>
<div class="container">
<div class="container">
    <img src="../pictures\logo.png" alt="Harlog logo">
    <h2 class="title">Tulemuste võrdlus:</h2>
    <form>
        <label for="groups">Vali esimene grupp</label>
        <!-- svelte-ignore a11y-no-onchange -->
        <select
            bind:value={selected1.group_id}
            on:change={() => {
                activity1 = true;
            }}
            
        >
            <option value={null}>Select group</option>
            {#await listGroup()}
                <p>Loading groups...</p>
            {:then groups}
                {#each groups as group}
                    <option value={group.group_id}>{group.group_name}</option>
                {/each}
            {/await}
        </select>
        {#if activity1}
            
            <!-- svelte-ignore a11y-no-onchange -->
            <select
                on:change={() => {
                    participant1 = true;                   
                }}
                on:change={selected1.activity_id = this.value}
            >
                <option value={null}>Select activity</option>
                {#await listActivities(selected1)}
                    <p>Loading activities...</p>
                {:then activities}
                    {#each activities as activity}
                        
                        <option value={activity.activity_id}
                            >{activity.activity_name}</option
                        >
                    {/each}
                {/await}
            </select>
        {/if}
        {#if participant1}
            <!-- svelte-ignore a11y-no-onchange -->
            <select on:change={selected1.part_id = this.value}>
                <option value={null}>Select participant </option>
                {#await listParticipants(selected1)}
                    <p>Loading participants...</p>
                {:then participants}
                    {#each participants as participant}
                        <option value={participant.part_id}
                            >{participant.part_name}</option
                        >
                    {/each}
                {/await}
            </select>
        {/if}
    </form>
    <button
        on:click={() => {
            window.location.reload();
        }}
    >Reset</button>
    <button
        on:click={() => {
            getComparisonData(selected1);
        }}
    >Lisa võrdlusesse</button>
</div>
<br>
<p style="margin-bottom: 130px; text-align: left; padding-left: 10px; color:#60495A; font-family: 'Electrolize', sans-serif; font-weight: bold; "></p>
<div class="resultcontainer">
    
    <table id="my-chart" class="charts-css column multiple data-spacing-3 show-labels show-primary-axis">
        <caption> Column Example #5 </caption>
        <tbody id="charts">
        
          
        </tbody>
    </table>
  
</div>
<hr />
<button on:click|preventDefault="{() => {push('/userpanel')}}">Tagasi</button>
<button on:click|preventDefault="{logout}">Logi välja</button>
</div>
<style>
    /*#column-example-5 {
        height: 40px;
        max-width: 300px;
        margin: 0 auto;
    }*/
    /*.onetime-container {  
	box-sizing: border-box;
    display: flex;
	/*display:block;*/
    /*flex-direction: column;
    /*max-width: 500px;*/
    /*margin: 0 auto;
	margin-left: 0;
	margin-right: 0;
	width: 900px;
	padding-left: 0;
	padding-right: 0;
	/*justify-content: center;*/
  	/*align-items: center;*/

	/*position: relative;
	
	  
	
	height: 100%;
	background-color: #FBFBFB;
	text-align: center;
    }*/
    .resultcontainer {
        display: flex;
        flex-direction: column;
        height: 80px;
        width: 360px;
        padding-left: 10px;
        padding-right: 10px;
    }	

    hr {
        width: 360px;
        margin-left: auto;
        margin-right: auto;
    }
    select{
        display: flex;
        flex-direction: column;
        margin: 0 auto;
        width: 360px;
        margin-right: 10px;
        margin-left: 10px;
        margin-top: 5px;
        margin-bottom: 5px;
    }
    #my-chart.column {
            height: 50px;
            max-width: 360px;
            color: #FBFBFB;
            
            margin: 0 auto;
            width: 435px;
            
            /*box-sizing: border-box;*/
            /*display: flex;
            /*display:block;*/
            /*flex-direction: column;
            /*max-width: 500px;*/
            
            /*margin-left: 0;
            margin-right: 0;
            /*width: 435px;
            padding-left: 0;
            padding-right: 0;
            /*justify-content: center;*/
            /*align-items: center;*/

            /*position: relative;
            
            
            
            
            background-color: #FBFBFB;
            text-align: center;*/
        }
        #my-chart {
  --color-1: #084C61;
  --color-2: #7f959c;
  --color-3: #1097C0;
  --color-4: rgba(230, 30, 30, 0.6);
  --color-5: rgba(230, 30, 30, 1.0);
  --color-6: rgba(230, 30, 30, 1.0);
  --color-7: rgba(230, 30, 30, 0.8);
  --color-8: rgba(230, 30, 30, 0.6);
  --color-9: rgba(230, 30, 30, 0.4);
  --color-10: rgba(230, 30, 30, 0.2);
}
        /* Multiple Datasets */
        /*#my-chart.multiple tbody tr td:nth-of-type(3n + 1) td {
            background-color: #084C61;
        }
        #my-chart.multiple tbody tr td:nth-of-type(3n + 2) {
            background-color: #7f959c;
        }
        #my-chart.multiple tbody tr td:nth-of-type(3n + 3) {
            background-color: #1097C0;
        }*/
        hr {
            width: 360px;
            margin-left: auto;
            -right: auto;
        /*height: 1px; 
        background-color: #BCABAE;*/
        
        }
        /*#my-chart tbody td {
            transform-origin: bottom;
            animation: revealing-bars 4s linear;
            animation-iteration-count: 1;
        }
        @keyframes revealing-bars {
            0%  { transform: scaleY( 0 ); }
            15% { transform: scaleY( 1 ); }
        }*/
       
</style>
