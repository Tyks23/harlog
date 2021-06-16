<script>
    import {push} from "svelte-spa-router";       
       
    let activity = {
    roomkey : sessionStorage.getItem('roomkey')
    };
    if(sessionStorage.getItem("token") == null) {
           
           // Redirect to login page
           push("/");
       }
    async function deleteRoomkey(){       
        const response = await fetch("http://localhost:3000/deleteroomkey", {
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
            
            sessionStorage.removeItem('roomkey');
            push("/userpanel");
        }
    }
    </script>
    <div class="container">
        <img src="../pictures\logo.png" alt="Harlog logo">
        <h2 class="title">Aktiivne küsitlus</h2>
        <p>Toa kood: </p>
        <h2 class="boxtext">{sessionStorage.getItem('roomkey')}</h2>

        <button on:click|preventDefault="{deleteRoomkey}">LÕPETA KÜSITLUS</button>
    </div>
    
    <style>
    
    
    .boxtext{       
        width: 180px;
        border: 15px solid #56A3A6;
        padding: 50px;
        margin: 10px;
        font-size: 30px;
        font-weight: bold;
        color: #084C61;
        border-radius: 10px;
        -moz-border-radius: 10px;
        -webkit-border-radius: 10px;
    }
    </style>