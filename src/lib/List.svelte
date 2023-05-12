<script>
import {onMount} from 'svelte';

let lists = JSON.parse(localStorage.getItem('lists')) || [];

let newList = "";

//On initialise la variable modal à false pour qu'elle ne soit pas affichée au chargement de la page
let modal = true;

function addToList(){
    if(newList === ""){
        alert("Please enter a todo item");
    }
    else{
        event.preventDefault();
        lists = [...lists, {id: lists.length + 1, name: newList}];
        newList = "";

        localStorage.setItem('lists', JSON.stringify(lists));
        modal = false;
    }
}

onMount(() => {
    const savedLists = localStorage.getItem('lists');
    if(savedLists){
        lists = JSON.parse(savedLists);
    }
});

function removeFromList(index){
    lists = lists.filter((_, i) => i !== index);
    localStorage.setItem('lists', JSON.stringify(lists));

}

</script>

<!-- Listes déjà existantes -->
<div class="lists">
    <h1 class="title-list">Lists</h1>
    <div class="line"></div>
    <ul>
        {#each lists as list, i}
            <li>
                <a href="/#/list/{list.id}">{list.name}</a>
                <button on:click={() => removeFromList(i)}>x</button>
            </li>
        {/each}
    </ul>

     <!--Au clique sur le bouton la modal devient true-->
    <button class="button-add-list" on:click={() =>(modal = !modal)} >+ New List</button>
</div>

<!--Si modal est true alors on affiche la modal-->
{#if modal}

<div class="modal-container">

<!--Au clique sur l'overlay la modal devient false et se ferme-->
<div class="overlay" on:click={() =>(modal = !modal)}></div>
<div class="modal">
    <!-- Bouton pour fermer la modal -->
    <button class="close-modal" on:click={() =>(modal = !modal)}>x</button>
    <h1>Add a list</h1>
    <label for="name">Name</label>
    <input  class="modal-input" type="text" bind:value={newList}>
    <button class="modal-btn-create-list" on:click={addToList} >Add</button>

</div>
</div>
{/if}

<style>
    .lists{
        margin: 30px 0 0 50px;
        width:200px;
    }

    .title-list{
        font-size: 23px;
        margin: 0 0 5px 15px;
        color:rgb(81, 80, 80);
    }

    .line{
        width: 100%;
        height: 1px;
        background-color: rgb(160, 160, 160);
        margin-bottom: 20px;
    }

    .lists ul{
        list-style: none;
        padding: 0;
        margin: 0;
    }

    .lists ul li{
        margin: 0 0 5px 15px;
    }

    .lists ul li a{
        text-decoration: none;
        color:rgb(81, 80, 80);
        font-size: 18px;
    }

    .lists ul li button{
        background-color: transparent;
        border: none;
        color: rgb(81, 80, 80);
        font-size: 18px;
        cursor: pointer;
    }

    .button-add-list{
        color: #3d7fe2;
        border: none;
        cursor: pointer;
        font-size: 18px;
        margin: 10px 0 5px 15px;
    }

    .button-add-list:hover{
        text-decoration:underline;
    }

    .modal-btn{
        background-color: #3d7fe2;
        color: white;
        border: none;
        border-radius: 5px;
        min-width: 150px;
        padding: 10px 14px;
        margin: 50px 20px;
        cursor: pointer;
    }

    .modal-btn:hover{
        background-color: #1567e2;
    }

    .modal-container{
        position: fixed;
        top: 0;
        width: 100vw;
        height:100vh;
    }

    .overlay{
        position: absolute;
        width: 100%;
        height: 100%;
        background-color: rgba(0,0,0,0.5);
    }

    .modal{
        width: 95%;
        max-width: 500px;
        min-width: 300px;
        height:200px;
        padding: 10px 15px 30px;
        background-color: white;
        position: absolute;
        top: 45%;
        left: 50%;
        transform: translate(-50%, -50%);
        border-radius: 5px;
        overflow: auto;
    }

    .modal h1{
        margin-bottom: 20px;
        font-size:20px
    }

    .close-modal{
        padding: 0px 10px 1px 10px;
        border: none;
        border-radius: 5px;
        font-size: 20px;
        position: absolute;
        top: 10px;
        right: 15px;
        background-color: #ff365e;
        color:white;
        cursor: pointer;
    }

    .close-modal:hover{
        background-color: #ff1e4a;
    }

    .modal-input{
        width: 100%;
        padding: 5px 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        margin-top:10px;
        margin-bottom: 40px;
    }
    
    .modal-input-button{
        border: 1px solid #ccc;
        border-radius: 5px;
        color:grey;
        width:19%;
        min-width: 80px;
        padding: 5px 5px;
        cursor: pointer;
    }

    .modal-input-subtask{
        margin-left: 20px;
        width: 70%;
        padding: 5px 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }
    
    .modal-input-button-subtask{
        border: 1px solid #ccc;
        border-radius: 5px;
        color:grey;
        width:19%;
        min-width: 80px;
        padding: 5px 3px;
        cursor: pointer;
    }

    .modal-input-button-close-subtask{
        border: 1px solid #ccc;
        border-radius: 5px;
        color:grey;
        width:3%;
        padding: 5px 3px;
        cursor: pointer;
    }


    .modal ul{
        margin: 10px 0 20px;
    }

    .modal ul li{
        margin-top: 3px;
    }

    .modal-btn-create-list{
        background-color: #3d7fe2;
        color: white;
        border: none;
        border-radius: 5px;
        display: block;
        position: absolute;
        bottom: 10px;
        right: 15px;
        min-width: 80px;
        padding: 5px 14px;
        cursor: pointer;
    }

    .modal-btn-create-list:hover{
        background-color: #1567e2;
    }

    .checked{
        text-decoration: line-through;
    }

    ul{
        margin:0;
        padding: 0;
    }

    li{
        list-style: none;
    }

    .liste-subtask{
        margin-left: 30px;
    }
</style>
