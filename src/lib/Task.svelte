<script>
    import { onMount } from 'svelte';

    export let params;
    let lists = JSON.parse(localStorage.getItem('lists')) || [];

    let list;

    //On initialise la variable modal à false pour qu'elle ne soit pas affichée au chargement de la page
    //let modal = true;

    //Variable pour le nouvel item de la liste
    let newTask = "";

    //Array pour les items de la liste
    let tasks= [
    ];

    onMount(() => {
        list = lists.find(list => list.id === parseInt(params.id));
    })

    //Fonction pour ajouter un item à la liste si non vide
    function addToList(){
        if(newTask === ""){
            alert("Please enter a todo item");
        }
        else{
            event.preventDefault();
            if (!list.tasks) {
                list.tasks = [];
            }
            else{
                list.tasks = [...list.tasks];
            }

            const task= {id: tasks.length + 1, name: newTask, completed : false};

            list.tasks.push(task);
            
            newTask = "";

            localStorage.setItem('lists', JSON.stringify(lists));

            console.log("liste tasks" + list.tasks);
        }
        
    }

    //Fonction pour supprimer un item de la liste
    function removeFromList(index){
        list.tasks.splice(index, 1);
        localStorage.setItem('lists', JSON.stringify(lists));
    }

    //Fonction pour cocher un item de la liste
    function checkItem(index){
        list.tasks[index].completed = !list.tasks[index].completed;
        localStorage.setItem('lists', JSON.stringify(lists));
    }

    console.log("lists"  + lists);
    console.log("list" + list);
    console.log("params id " + params.id);
</script>


<div>
    
</div>
<!--Si modal est true alors on affiche la modal-->
<div class="modal-container">
    <!--Au clique sur l'overlay la modal devient false et se ferme-->
    <div class="modal">
        <!-- Bouton pour fermer la modal -->
        {#if list}
        <h1>{list.name}</h1>
        {/if}
        <div class="line"></div>
        <!-- Input pour ajouter un item à la liste -->
        <form on:submit={addToList}>
            <input class="modal-input" type="text" bind:value={newTask} placeholder="+    Add task">
        </form>
        <!-- <input class="modal-input" type="text" bind:value={newTask} placeholder="new todo item"> -->
        <!-- <button class="modal-input-button" on:click={addToList} >Add task</button> -->
    
        <!--for each pour ajouter chaque item à la liste-->
        {#if list && list.tasks}
        <ul>
            <!-- Afficher les sous taches -->
        {#each list.tasks as item, index}
          <li class:checked={item.completed}>
            <input type="checkbox" on:click={() => checkItem(index)} bind:checked={item.completed}/> {item.name}
            <!-- <button on:click={() =>(subTask = !subTask)}>+</button> -->
            <button on:click={() => removeFromList(index)}>x</button>
          </li>
        {:else}
          Ajouter un item à la liste
        {/each}
        </ul>
        {/if}
    </div>
</div>

<style>
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
        width: 100%;
        max-width: 500px;
        min-width: 300px;
        height:400px;
        max-height:400px;
        padding: 10px 15px 30px;
        background-color: white;
        position: absolute;
        top: 53%;
        left: 50%;
        transform: translate(-50%, -50%);
        border-radius: 5px;
        overflow: auto;
    }

    .modal h1{
        margin-bottom: 0px;
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
        padding: 10px 10px;
        border: none;
        background-color: rgb(243, 240, 240);
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
