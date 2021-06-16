
    

<script async> 
    console.log(sessionStorage);
    if(sessionStorage.getItem("token") == null) {
           
           // Redirect to login page
           push("/");
       }
    import {push, replace} from "svelte-spa-router";
    

    let group = {
        group_name : undefined
    }
    let activity= {
        incognito : false
    }

   
    function logout(){
        sessionStorage.removeItem("token");
        push("/");
    }
    
    async function createGroup(){
        let haveErrors2 = false;
        if(typeof group.group_name === 'undefined'){
            document.getElementById('error-creategroup').innerHTML = "Sisesta grupp!"
            haveErrors2= true;
        }
        else{
            document.getElementById('error-creategroup').innerHTML = ""
        }
        if(haveErrors2 === false){
            const response = await fetch("http://localhost:3000/creategroup", {
                method: 'POST', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json',
                'Authorization' : sessionStorage.getItem('token')
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: 'follow', // manual, *follow, error
                referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(group) // body data type must match "Content-Type" header
            });
            if(response.ok){
                let data = await response.json();
                let groupList = document.getElementsByClassName('groupList')[0];
                let appendable = document.createElement('li'); 
                let button = document.createElement('button');
                button.value = data.group_id;
                button.textContent = data.group_name;
                button.onclick = () => {createActivity(data.group_id)};    
                appendable.appendChild(button);
                groupList.appendChild(appendable);
                    //push("/");
            }
        }
        haveErrors2 = false;    
    }
    
    async function createActivity(group_id){
    
        console.log(activity);
        console.log(activity.activity_name);
        let haveErrors = false;
        if(typeof activity.activity_name === 'undefined'|| activity.activity_name === ""){
            document.getElementById('error-activity').innerHTML = "Sisesta tegevus!"
            haveErrors= true;
        }
        else{
            document.getElementById('error-activity').innerHTML = ""
        }
        if(typeof activity.roomkey === 'undefined' || activity.roomkey === "" ){
            document.getElementById('error-key').innerHTML = "Sisesta toa võti!"
            haveErrors= true;
        }
        else{
            document.getElementById('error-key').innerHTML = ""
        }
        if(haveErrors === false){
            
            activity.group_id = group_id;
            const response = await fetch("http://localhost:3000/createactivity", {
                method: 'POST', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json',
                'Authorization' : sessionStorage.getItem('token')
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: 'follow', // manual, *follow, error
                referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(activity) // body data type must match "Content-Type" header
            });
            if(response.ok){
                
                sessionStorage.setItem('roomkey', activity.roomkey);
                push("/activesession");
            }
        }   
        
        haveErrors =  false;  
    }

    async function listGroup(){
        const response = await fetch("http://localhost:3000/listgroup", {
            method: 'GET', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'same-origin', // include, *same-origin, omit
            headers: {
            'Content-Type': 'application/json',
            'Authorization' : sessionStorage.getItem('token')
            // 'Content-Type': 'application/x-www-form-urlencoded',
            },
            redirect: 'follow', // manual, *follow, error
            referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
        });
        
       
        if(response.ok){
            return await response.json();
            //push("/");
        }
    }
    </script>
    <div class="container">
        <img src="../pictures\logo.png" alt="Harlog logo">
        <form class = "container">
            <h2 class="title">Alusta sessiooni:</h2>
            <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Tunni tegevus:</p>
            <input type="text" placeholder="Tegevuse nimi" bind:value="{activity.activity_name}"><span class="errormsg" id="error-activity"></span>
            <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Ruumi võti:</p>
            <input type="text" placeholder="Võti" bind:value="{activity.roomkey}"><span class="errormsg" id="error-key"></span>
            <!--<div style="padding-left: 10px; padding-right: 10px; text-align: left; margin-top: 5px; margin-bottom: 5px;" class="switchspan">
                <label class="switch">
                    
                    <input type="checkbox"on:click="{() => {activity.incognito = !activity.incognito}}">
                    <span class="slider round"></span>
                </label>
                <span>Anonüümne sessioon &nbsp </span>
            
            </div> --->
        </form>
        <form class= "container">
            <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Vali grupp, millega tegevust alustada:</p>
            <div class="table"> 
                <ul class = "groupList">
                    {#await listGroup()}
                    <p>Loading groups...</p>
                    {:then groups}
                    {#each groups as group}
                
                    <button on:click|preventDefault="{() => {createActivity(group.group_id)}}">{group.group_name}</button>
                    {/each}
                    {/await}                
                </ul>
            </div>  
            <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Uue grupi loomine:</p>  
            <input type="text" bind:value="{group.group_name}"><span class="errormsg" id="error-creategroup"></span> 
            <button on:click|preventDefault="{createGroup}">Loo grupp</button>
            <hr />
            <button on:click|preventDefault="{() => {push('/userpanel')}}">Tagasi</button> 
            <button on:click|preventDefault="{logout}">Logi välja</button>
            
            
        </form>
    </div>
    
    
    <style>
    /*.container {  
        display: flex;
        flex-direction: column;
        
    }*/
    ul {
       display: flex;
       margin: auto;
       
        flex-direction: column;
        list-style-type: none;
        padding-left: 0%;
        /*width: 90%; 
        list-style-type:none;
        margin:auto;
        padding:0;
        position:relative;
        left:5%;
        width: 100%;
        list-style: none;*/
    }
    /* The switch - the box around the slider */
.switch {
  position: relative;
  display: inline-block;
  width: 40px;
  height: 25px;
  vertical-align: text-bottom;

}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #BCABAE;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 19px;
  width: 19px;
  left: 3px;
  bottom: 3px;
  background-color: #FBFBFB;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #60495A;
}

input:focus + .slider {
  box-shadow: 0 0 3px #60495A;
}

input:checked + .slider:before {
  -webkit-transform: translateX(15px);
  -ms-transform: translateX(15px);
  transform: translateX(15px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 25px;
  
}

.slider.round:before {
  border-radius: 50%;
  
}
.switchspan{
    display:inline;
    
}   
hr {
    width: 360px;
    margin-left: auto;
    margin-right: auto;
}            
    </style>