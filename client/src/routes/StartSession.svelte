<script>  
    import {push} from "svelte-spa-router";
    </script>
    <div class="container">
        <h1>Harlog</h1>
        <form>
            <h1 class="title">Alusta küsitlust:</h1>
            <p>Tegevuse nimi:</p>
            <input type="text" placeholder="Tegevuse nimi">
        </form>
        <form>
            <p>Vali grupp:</p>
            <ul>
                <li><input type="radio" name="group"> Example group 1</li>
                <li><input type="radio" name="group"> Example group 2</li>
                <li><input type="radio" name="group"> Example group 3</li>
                <li><input type="radio" name="group"> Example group 4</li>
                <li><input type="text"> </li>
                <li><button>LOO GRUPP</button></li>
            </ul>       
            <button on:click|preventDefault="{() => {push('/activesession')}}">ALUSTA KÜSITLUST</button>
        </form>
    </div>
    
    
    <style>
    .container {  
        display: flex;
        flex-direction: column;
    }
        
    </style>