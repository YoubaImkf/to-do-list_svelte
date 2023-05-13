<script>
    import { onMount } from 'svelte';
    import FormSubTask from './FormSubTask.svelte';

    export let params;
    let lists = JSON.parse(localStorage.getItem('lists')) || [];

    let list;

    //On initialise la variable list à false pour qu'elle ne soit pas affichée au chargement de la page
    let inputSubTask = false;

    //On initialise la variable inputRenameList à false pour qu'elle ne soit pas affichée au chargement de la page
    let inputRenameList = false;

    //Variable pour le nouvel item de la liste
    let newTask = "";

    //Array pour les items de la liste
    let tasks= [
    ];

    //Variable pour la nouvelle sous-tâche
    let newSubTask = "";

    //Array pour les sous-tâches
    let subTasks;

    onMount(() => {
        list = lists.find(list => list.id === parseInt(params.id));
        console.log(list);
    })

    //Renommer la liste puis enregistrer en base de données
    function renameList(){
        if(list.name === ""){
            alert("Please enter a list name");
        }
        else{
            event.preventDefault();
            list.name = list.name;
            localStorage.setItem('lists', JSON.stringify(lists));
            inputRenameList = false;
        }
    }

    //Fonction pour ajouter un item à la liste si non vide
    function addTaskToList(){
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

            const task= {id: tasks.length + 1, name: newTask, completed : false, subTasks: []};

            list.tasks.push(task);
            
            newTask = "";

            localStorage.setItem('lists', JSON.stringify(lists));
        }
        
    }

    //Fonction pour supprimer un item de la liste
    function removeFromList(index){
        list.tasks = list.tasks.filter((_, i) => i !== index);
        localStorage.setItem('lists', JSON.stringify(lists));
    }

    //Fonction pour cocher un item de la liste
    function checkItem(index){
        list.tasks[index].completed = !list.tasks[index].completed;
        localStorage.setItem('lists', JSON.stringify(lists));
    }

    //Fonction pour dupliquer une liste avec ses tâches

    function duplicateList(){
        lists = [...lists, {id: lists.length + 1, name: list.name + " (copie)", tasks: list.tasks}];
        localStorage.setItem('lists', JSON.stringify(lists));
        alert("List duplicated");
    }

    //Fonction pour ajouter une sous-tâche
    function addSubTaskToList(index){
        if(newSubTask === ""){
            console.log("Please enter a subtask");
        }
        else{
            console.log(list.tasks[index]);
            event.preventDefault();
            if (!list.tasks[index].subTasks) {
                list.tasks[index].subTasks = [];
            }
            else{
                list.tasks[index].subTasks = [...list.tasks[index].subTasks];
            }

            const subTask= {id: subTasks.length + 1, name: newSubTask, completed : false};

            list.tasks[index].subTasks.push(subTask);
            
            newSubTask = "";

            localStorage.setItem('lists', JSON.stringify(lists));

            console.log("liste subtasks" + list.tasks[index].subTasks);

            return list.tasks[index].subTasks;
        }
        
    }
    

    //Fonction pour cocher une sous-tâche
    function checkSubItem(index, subIndex){
        list.tasks[index].subTasks[subIndex].completed = !list.tasks[index].subTasks[subIndex].completed;
        localStorage.setItem('lists', JSON.stringify(lists));
    }

    //Fonction pour supprimer une sous-tâche
    function removeFromSubList(index, subIndex){
        list.tasks[index].subTasks.splice(subIndex, 1);
        localStorage.setItem('lists', JSON.stringify(lists));
    }

    //Fonction pour afficher le formulaire d'ajout d'une sous-tâche

    function showSubTaskForm(index){
        let input = document.getElementById("subTaskForm"+index);

        console.log("Afficher l'input du formulaire : " + input);
        console.log("Afficher l'index : " + index);

        input.style.display = "block";
    }

</script>

<nav class="nav-bar">
    <a class="title" href="/">Back to lists</a>
    <div class="line"></div>
</nav>

<div class="list-container">
    <!--Au clique sur l'overlay la list devient false et se ferme-->
    <div class="list">
        <!-- Bouton pour fermer la list -->
        {#if list}
            {#if inputRenameList === false}
            <div class="list-name">
                <h1>{list.name}</h1>
                <!-- Renommer la liste -->
                <button class="button-list" on:click={() =>(inputRenameList = !inputRenameList)}>Rename list</button>
            </div>
            {:else}
            <form on:submit={renameList}>
                <input class="list-input" type="text" bind:value={list.name} placeholder="Rename list">
            </form>
            {/if}
        {/if}

        <div class="line float"></div>
        <!-- Input pour ajouter un item à la liste -->
        <form on:submit={addTaskToList}>
            <input class="list-input" type="text" bind:value={newTask} placeholder="+    Add task">
        </form>
        <!-- <input class="list-input" type="text" bind:value={newTask} placeholder="new todo item"> -->
        <!-- <button class="list-input-button" on:click={addTaskToList} >Add task</button> -->
    
        <!--for each pour ajouter chaque item à la liste-->
        {#if list && list.tasks}
        <ul>
            <!-- Afficher les sous taches -->
        {#each list.tasks as item, index}
          <li class:checked={item.completed}>
            <input type="checkbox" on:click={() => checkItem(index)} bind:checked={item.completed}/> {item.name}
            <!-- <button on:click={() =>(inputSubTask = !inputSubTask)}>+</button> -->
            <button on:click={() =>(showSubTaskForm(index))}>+</button>
            <button on:click={() => removeFromList(index)}>x</button>

            <form id="subTaskForm{index}" style="display:none" on:submit={addSubTaskToList(index)}>
                <input class="list-input" type="text" bind:value={newSubTask} placeholder="+    Add subtask">
            </form>

            <!-- {#if inputSubTask}
            <form  on:submit={addSubTaskToList}>
                <input class="list-input" type="text" bind:value={newSubTask} placeholder="+    Add subtask">
            </form>
            {/if} -->
            <!-- {#if list.tasks[index].subTasks}
            <ul>
                {#each list.tasks[index].subTasks as subItem, subIndex}
                    <li class:checked={subItem.completed}>
                        <input type="checkbox" on:click={() => checkSubItem(index, subIndex)} bind:checked={subItem.completed}/> {subItem.name}
                        <button on:click={() => removeFromSubList(index, subIndex)}>x</button>
                    </li>
                {/each}
            </ul>
            {/if} -->
          </li>
        {:else}
          Ajouter un item à la liste
        {/each}
        </ul>
        {/if}
        <!-- Bouton qui permet de dupliquer la liste -->
        <button class="button-list" on:click={duplicateList}>Duplicate list</button>
    </div>
</div>

<style>

    /* .list-container{
        position: fixed;
        top: 0;
        width: 100vw;
        height:100vh;
    } */


    .nav-bar{
        margin: 30px 0 0 50px;
        width:200px;
    }

    .title{
        text-decoration: none;
        font-size: 23px;
        margin: 0 0 5px 15px;
        color:rgb(81, 80, 80);
        font-weight: bold;
    }

    .list{
        width: 100%;
        max-width: 500px;
        min-width: 300px;
        height:auto;
        max-height:400px;
        padding: 10px 15px 30px;
        background-color: white;
        position: absolute;
        top: 45%;
        left: 50%;
        transform: translate(-50%, -50%);
        border-radius: 5px;
        overflow: auto;
    }

    .list-name{
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 10px;
    }

    .list h1{
        font-size:20px;
    }

    .button-list{
        border: 1px solid #ccc;
        border-radius: 5px;
        color:grey;
        width:20%;
        margin-top:3px;
        padding: 5px 3px;
        cursor: pointer;
    }

    .list-input{
        width: 100%;
        padding: 10px 10px;
        border: none;
        background-color: rgb(243, 240, 240);
        border-radius: 5px;
        margin-bottom: 10px;

    }
    

    .list-input-subtask{
        margin-left: 20px;
        width: 70%;
        padding: 5px 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
    }

    .list-input-button-close-subtask{
        border: 1px solid #ccc;
        border-radius: 5px;
        color:grey;
        width:3%;
        padding: 5px 3px;
        cursor: pointer;
    }


    .list ul{
        margin: 10px 0 20px;
    }

    .list ul li{
        margin-top: 3px;
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
