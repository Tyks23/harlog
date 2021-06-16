<script>
    import 'charts.css';
    import { push } from "svelte-spa-router";
    if(sessionStorage.getItem("token") == null) {
           
        // Redirect to login page
        push("/");
    }
    function logout(){
        sessionStorage.removeItem("token");
        push("/");
    }
let selectedActivity = {
    activity_id : sessionStorage.getItem('activity_id')
};

let participants = {};

    let data;

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
        let counter = 0;

        const newArray = array.reduce((acc, cur) => {
            counter += stop + 1 - start;
            return [...acc, ...cur.slice(start, stop + 1)];
        }, []);

        if (isReversed != undefined)
            newArray.forEach((x, i) => {
                if (isReversed[i % isReversed.length]) {
                    newArray[i] = 6 - x;
                }
            });

        return newArray.reduce((acc, cur) => acc + cur, 0) / counter;
    };

(async function getParticipants() {
        const response = await fetch("http://localhost:3000/getparticipants", {
            method: "POST", // *GET, POST, PUT, DELETE, etc.
            mode: "cors", // no-cors, *cors, same-origin
            cache: "no-cache", // *default, no-cache, reload, force-cache, only-if-cached
            credentials: "same-origin", // include, *same-origin, omit
            headers: {
                "Content-Type": "application/json",
                //'Authorization' : sessionStorage.getItem('token')
                // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: "follow", // manual, *follow, error
            referrerPolicy: "no-referrer", // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
            body: JSON.stringify(selectedActivity), // body data type must match "Content-Type" header
        }      
    )
    if (response.ok) {
            data = (await response.json()).rows;
            console.log(data);
            participants = Array.from(
                new Set(data.map((item) => item.part_id))
            ).reduce(
                (acc, curr) => ({ ...acc, [curr]: { name: "", answers: [] } }),
                {}
            );
            data.forEach(({ part_id, answer, part_name }) => {
                //console.log(row);
                participants[part_id].answers.push(answer);
                participants[part_id].name = part_name;
            });

            let counter = 0;

            Object.entries(participants).forEach(([, value]) => {
                const { answers } = value;

                value.emotion1 = getAvgOfCols(answers, 0, 1, [false, true]);
                value.emotion2 = getAvgOfCols(answers, 2, 3, [false, true]);
                value.emotion3 = getAvgOfCols(answers, 4, 5);
            });

            

            console.log(activityEmotions);
            activityEmotions.emotion1 /= counter;
            activityEmotions.emotion2 /= counter;
            activityEmotions.emotion3 /= counter;
            console.log(activityEmotions);
        }})();


</script>
<div class="container">
    <img src="../pictures\logo.png" alt="Harlog logo">
    <h2>Tegevuse tulemused</h2>
    <p class="pageinfo">Grupp: {sessionStorage.group_name}</p>
    <p class="pageinfo">Tegevus: {sessionStorage.activity_name}</p>
    {#each Object.keys(participants) as participant}
        <hr />
        <p style="margin-bottom: 130px; text-align: left; padding-left: 10px; color:#60495A; font-family: 'Electrolize', sans-serif; font-weight: bold;">Nimi: {participants[participant].name}</p>
        <div class="resultcontainer">
        <table id="my-chart" class="charts-css column show-labels data-spacing-20 show-primary-axis ">
            <caption> Bar Example #6 </caption>
            <thead>
                <tr>
                    <th scope="col"> Year </th>
                    <th scope="col"> Progress </th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <th scope="row" style="color: #084C61; font-size: 75%;">Käitumuslik</th>
                    <td style="--size:{participants[participant].emotion1};">
                        <span class="data"
                            >{participants[participant].emotion1.toFixed(2)}</span
                        >
                    </td>
                </tr>
                <tr>
                    <th scope="row" style="color: #084C61; font-size: 75%;">Emotsionaalne</th>
                    <td style="--size:{participants[participant].emotion2}">
                        <span class="data"
                            >{participants[participant].emotion2.toFixed(2)}</span
                        >
                    </td>
                </tr>
                <tr>
                    <th scope="row" style="color: #084C61; font-size: 75%;">Kognitiivne</th>
                    <td style="--size:{participants[participant].emotion3}">
                        <span class="data"
                            >{participants[participant].emotion3.toFixed(2)}</span
                        >
                    </td>
                </tr>
            </tbody>
        </table>
        </div>
        <button on:click="{() => {sessionStorage.setItem('part_id', participant), sessionStorage.setItem('part_name', participants[participant].name); push('/resultsparticipant');}}">OSALEJA TULEMUSED</button>
        
    {/each}
    <hr />
    <button on:click|preventDefault="{() => {push('/resultsgroup')}}">Tagasi</button>
    <button on:click|preventDefault="{logout}">Logi välja</button>
</div>

<style>
    .resultcontainer {
        display: flex;
        flex-direction: column;
        height: 80px;
        width: 360px;
        padding-left: 10px;
        padding-right: 10px;
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
    /* Single Dataset */
    #my-chart:not(.multiple) tbody tr:nth-of-type(3n + 1) td {
        background-color: #084C61;
    }
    #my-chart:not(.multiple) tbody tr:nth-of-type(3n + 2) td {
     background-color: #0C7292;
    }
    #my-chart:not(.multiple) tbody tr:nth-of-type(3n + 3) td {
        background-color: #1097C0;
    }
    hr {
    width: 360px;
    margin-left: auto;
    margin-right: auto;
}
#my-chart tbody td {
        transform-origin: bottom;
        animation: revealing-bars 4s linear;
        animation-iteration-count: 1;
    }
    @keyframes revealing-bars {
        0%  { transform: scaleY( 0 ); }
        15% { transform: scaleY( 1 ); }
    }
</style>