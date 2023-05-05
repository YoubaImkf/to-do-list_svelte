<script>

    //On initialise la variable modal à false pour qu'elle ne soit pas affichée au chargement de la page
    let modal = true;

    //Variable pour le nouvel item de la liste
    let newItem = "";

    //Array pour les items de la liste
    let list = [
        {id: 1, name: "Create the todolist", completed : false},
        {id: 2, name: "Add some design", completed : false},
        {id: 3, name: "Do a push", completed : false},
    ];

    //Fonction pour ajouter un item à la liste si non vide
    function addToList(){
        if(newItem === ""){
            alert("Please enter a todo item");
        }
        else{
            list = [...list, {id: list.length + 1, name: newItem, completed : false}];
            newItem = "";
        }
        
    }

    //Trouver un moyen d'add list item en appuyant sur la touche enter

    // let inputItem = document.getElementById("inputItem");

    // inputItem.addEventListener("keypress", function(event) {
    //     if (event.key === 'Enter') {
    //         event.preventDefault();
    //         addToList();
    //     }
    // });

    //Fonction pour supprimer un item de la liste
    function removeFromList(index){
        list = list.filter((item, i) => i !== index);
    }



</script>

<!--Au clique sur le bouton la modal devient true-->

<button class="modal-btn" on:click={() =>(modal = !modal)} >Create a new list</button>

<!--Si modal est true alors on affiche la modal-->
{#if modal}
<div class="modal-container">
    <!--Au clique sur l'overlay la modal devient false et se ferme-->
    <div class="overlay" on:click={() =>(modal = !modal)}></div>
    <div class="modal">
        <!-- Bouton pour fermer la modal -->
        <button class="close-modal" on:click={() =>(modal = !modal)}>x</button>
        <h1>Liste 1</h1>
        <!-- Input pour ajouter un item à la liste -->
        <input class="modal-input" type="text" bind:value={newItem} placeholder="new todo item">
        <button class="modal-input-button" on:click={addToList} >Add task</button>
    
        <!--for each pour ajouter chaque item à la liste-->
        <ul>
        {#each list as item, index}
          <li class:checked={item.completed}>
            <input type="checkbox" bind:checked={item.completed}/> {item.name} <button on:click={() => removeFromList(index)}>x</button>
          </li>
        {:else}
          Ajouter un item à la liste
        {/each}
        </ul>
        <button class="modal-btn-create-list">Add list</button>
    </div>
</div>
{/if}

<style>
    .modal-btn{
        background-color: #3d7fe2;
        color: white;
        border: none;
        border-radius: 5px;
        display: block;
        min-width: 150px;
        padding: 10px 14px;
        margin: 100px auto;
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
        padding: 10px 15px 30px;
        background-color: white;
        position: absolute;
        top: 45%;
        left: 50%;
        transform: translate(-50%, -50%);
        border-radius: 5px;
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
        width: 80%;
        padding: 5px 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        margin-bottom: 10px;
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
</style>
