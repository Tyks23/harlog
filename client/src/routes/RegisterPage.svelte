<meta name="viewport" content="width=device-width, initial-scale=1.0">
<script>
    import {push} from "svelte-spa-router";
    let user = {
     
    }
    let passwordVal;

    async function register(){
        let haveErrors = false;
        if(typeof user.name === 'undefined' || user.name.length <5 || user.name.length > 15){
            haveErrors = true;
            document.getElementById('error-username').innerHTML = " Palun sisestage kasutajanimi(5-15 tähemärki)!" 
            //alert('Palun sisestage kasutajanimi(5-15 tähemärki)!') 
        }
        else{
            document.getElementById('error-username').innerHTML = ""; 
        }
        if(typeof user.password === 'undefined'|| user.password === ""){
            haveErrors = true;
            document.getElementById('error-password').innerHTML = " Palun sisestage parool!"
        }
        else{
            document.getElementById('error-password').innerHTML = "" 
        }
        if(typeof user.password !== 'undefined' && user.password.length < 7){
            haveErrors = true;
            document.getElementById('error-password2').innerHTML = " Parool peab olema vähemalt 7 tähemärki pikk!"
        }
        else{
            document.getElementById('error-password2').innerHTML = "" 
        }
        if(user.password !== passwordVal){
            haveErrors = true;
            document.getElementById('error-passwordval').innerHTML = " Sisestatud paroolid ei klapi!"
        }
        else{
            document.getElementById('error-passwordval').innerHTML = ""
        }
        if(typeof user.email === 'undefined' || user.email === ""){
            haveErrors = true;
            document.getElementById('error-email').innerHTML = " Palun sisestage meiliaadress!"
        }
        else{
            
        
            const re = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
            console.log(re.test(user.email)); 
            if (re.test(user.email) === false){
                haveErrors = true;
                document.getElementById('error-email').innerHTML = " Pole korrektne meiliaadress!"
            }
            else{
                document.getElementById('error-email').innerHTML = "" 
            }
        }      
        
            
        console.log(user);
        console.log(JSON.stringify(user));
        if (haveErrors ===false){
            const response = await fetch("http://localhost:3000/register", {
                method: 'POST', // *GET, POST, PUT, DELETE, etc.
                mode: 'cors', // no-cors, *cors, same-origin
                cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
                credentials: 'same-origin', // include, *same-origin, omit
                headers: {
                'Content-Type': 'application/json'
                // 'Content-Type': 'application/x-www-form-urlencoded',
                },
                redirect: 'follow', // manual, *follow, error
                referrerPolicy: 'no-referrer', // no-referrer, *no-referrer-when-downgrade, origin, origin-when-cross-origin, same-origin, strict-origin, strict-origin-when-cross-origin, unsafe-url
                body: JSON.stringify(user) // body data type must match "Content-Type" header
            });
            if(response.ok){
                push("/");
            }
        }
        haveErrors = false;


    }
    </script>
    <div class="container">
        <img src="../pictures\logo.png" alt="Harlog logo"> 
        <h2 class="title">Registreeru</h2>
        <form class="container">
        <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta kasutajatunnus:</p>
        <input type="text" placeholder="Kasutajatunnus" bind:value="{user.name}"><span class="errormsg" id="error-username"></span>
        <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta email:</p>
        <input type="text" placeholder="Email" bind:value="{user.email}"><span class="errormsg" id="error-email"></span><span class="errormsg" id="error-email2"></span>
        <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta parool:</p>
        <input type="password" placeholder="Parool" bind:value="{user.password}"><span class="errormsg" id="error-password"></span>
        <span class="errormsg" id="error-password2"></span>
        <p style="padding-left: 10px; padding-right: 10px; text-align: left;">Sisesta uuesti parool:</p>
        <input type="password" placeholder="Parool" bind:value="{passwordVal}"><span class="errormsg" id="error-passwordval"></span>
        <input type="submit" value="REGISTREERU" on:click|preventDefault="{register}">   
        <button on:click|preventDefault="{() => {push('/')}}">Pealehele</button>
        </form>
    </div>
    
    <style>
        
    
        
    </style>