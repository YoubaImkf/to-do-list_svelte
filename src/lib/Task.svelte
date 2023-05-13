<script>
    import { onMount } from "svelte";
    import List from "./List.svelte";

    export let params;
    let lists = JSON.parse(localStorage.getItem("lists")) || [];

    let list;

    //On initialise la variable list à false pour qu'elle ne soit pas affichée au chargement de la page
    let inputSubTask = false;

    //On initialise la variable inputRenameList à false pour qu'elle ne soit pas affichée au chargement de la page
    let inputRenameList = false;

    //Variable pour le nouvel item de la liste
    let newTask = "";

    //Array pour les items de la liste
    let tasks = [];

    //Variable pour la nouvelle sous-tâche
    let newSubTask = "";

    //Array pour les sous-tâches
    let subTasks = [];

    onMount(() => {
        list = lists.find((list) => list.id === parseInt(params.id));
    });

    //Renommer la liste puis enregistrer en base de données
    function renameList() {
        if (list.name === "") {
            alert("Please enter a list name");
        } else {
            event.preventDefault();
            list.name = list.name;
            localStorage.setItem("lists", JSON.stringify(lists));
            inputRenameList = false;
        }
    }

    //Fonction pour ajouter un item à la liste si non vide
    function addTaskToList() {
        if (newTask === "") {
            alert("Please enter a todo item");
        } else {
            event.preventDefault();
            if (!list.tasks) {
                list.tasks = [];
            } else {
                list.tasks = [...list.tasks];
            }

            const task = {
                id: tasks.length + 1,
                name: newTask,
                completed: false,
            };

            list.tasks.push(task);

            newTask = "";

            localStorage.setItem("lists", JSON.stringify(lists));

            console.log("liste tasks" + list.tasks);
        }
    }

    //Fonction pour supprimer un item de la liste
    function removeFromList(index) {
        list.tasks = list.tasks.filter((_, i) => i !== index);
        localStorage.setItem("lists", JSON.stringify(lists));
    }

    //Fonction pour cocher un item de la liste
    function checkItem(index) {
        list.tasks[index].completed = !list.tasks[index].completed;
        localStorage.setItem("lists", JSON.stringify(lists));
    }

    //Fonction pour dupliquer une liste avec ses tâches

    function duplicateList() {
        lists = [
            ...lists,
            {
                id: lists.length + 1,
                name: list.name + " (copie)",
                tasks: list.tasks,
            },
        ];
        localStorage.setItem("lists", JSON.stringify(lists));
        alert("List duplicated");
    }

    //Les fonctions ci dessous portent sur les sous-tâches

    //Fonction pour ajouter une sous-tâche
    function addSubTaskToList(index) {
        if (newSubTask === "") {
            alert("Please enter a subtask");
        } else {
            event.preventDefault();
            if (!list.tasks[index].subTasks) {
                list.tasks[index].subTasks = [];
            } else {
                list.tasks[index].subTasks = [...list.tasks[index].subTasks];
            }

            const subTask = {
                id: subTasks.length + 1,
                name: newSubTask,
                completed: false,
            };

            list.tasks[index].subTasks.push(subTask);

            newSubTask = "";

            localStorage.setItem("lists", JSON.stringify(lists));

            console.log("liste subtasks" + list.tasks[index].subTasks);
        }
    }

    //Fonction pour cocher une sous-tâche
    function checkSubItem(index, subIndex) {
        list.tasks[index].subTasks[subIndex].completed =
            !list.tasks[index].subTasks[subIndex].completed;
        localStorage.setItem("lists", JSON.stringify(lists));
    }

    //Fonction pour supprimer une sous-tâche
    function removeFromSubList(index, subIndex) {
        list.tasks[index].subTasks.splice(subIndex, 1);
        localStorage.setItem("lists", JSON.stringify(lists));
    }
</script>

<List/>

<div class="list-container">

    <!--Au clique sur l'overlay la list devient false et se ferme-->
    <div class="list">
        <!-- Bouton pour fermer la list -->
        {#if list}
            {#if inputRenameList === false}
                <div class="list-name">
                    <h1 > 
                        <!-- Renommer la liste -->
                        <button class="button-list" on:click={() =>(inputRenameList = !inputRenameList)}> 
                            {list.name} 
                        </button>
                    </h1>
                </div>
            {:else}
                <form on:submit={renameList}>
                    <input
                        class="modifying-list-name"
                        type="text"
                        bind:value={list.name}
                        placeholder="Rename list"
                    />
                </form>
            {/if}
        {/if}

        <div class="line" />
        <!-- Input pour ajouter un item à la liste -->
        <form on:submit={addTaskToList}>
            <input
                class="list-input"
                type="text"
                bind:value={newTask}
                placeholder="        Add task"
            />
        </form>
        <!-- <input class="list-input" type="text" bind:value={newTask} placeholder="new todo item"> -->
        <!-- <button class="list-input-button" on:click={addTaskToList} >Add task</button> -->

        <!--for each pour ajouter chaque item à la liste-->
        {#if list && list.tasks}
            <ul class="task-list">
                <!-- Afficher les sous taches -->
                {#each list.tasks as item, index}
                    <li class="task-item" class:checked={item.completed}>
                        <input
                            class="checkbox-round"
                            type="checkbox"
                            on:click={() => checkItem(index)}
                            bind:checked={item.completed}
                        />
                        {item.name}
                        <!-- <button on:click={() =>(inputSubTask = !inputSubTask)}>+</button> -->
                        <button
                            class="remove-task"
                            on:click={() => removeFromList(index)}
                        >
                            <img src="src\assets\close.svg" alt="" />
                        </button>
                        <!-- {#if inputSubTask}
            <form on:submit={addSubTaskToList}>
                <input class="list-input" type="text" bind:value={newSubTask} placeholder="+    Add subtask">
            </form>

            {#if list.tasks[index].subTasks}
            <ul>
                {#each list.tasks[index].subTasks as subItem, subIndex}
                    <li class:checked={subItem.completed}>
                        <input type="checkbox" on:click={() => checkSubItem(index, subIndex)} bind:checked={subItem.completed}/> {subItem.name}
                        <button on:click={() => removeFromSubList(index, subIndex)}>x</button>
                    </li>
                {/each}
            </ul>
            {/if}
            {/if} -->
                    </li>
                {:else}
                    Ajouter un item à la liste
                {/each}
            </ul>
        {/if}
        <!-- Bouton qui permet de dupliquer la liste -->
        <button class="button-list" on:click={duplicateList}
            >Duplicate list</button
        >
    </div>
</div>

<style>
    .list-input::placeholder {
        color: #c6c6c6;
        font-size: 20px;
    }

    .line {
        width: 65%;
        height: 1px;
        background-color: #dddddd;
        margin-bottom: 40px;
    }

    .underline {
        width: 200px;
        height: 1px;
        background-color: rgb(225, 224, 224);
    }

    .title {
        text-decoration: none;
        font-size: 16px;
        color: rgb(81, 80, 80);
        font-weight: bold;
    }

    .list {
        max-width: 100%;
        margin-left: 26%;
    }

    .list-name {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 28px;
    }
/* 
    .button-list{
        margin-top: 50px;
    } */

    .list h1 button{
        margin-top: 50px;
        font-size: 24px;
        font-weight: 700;
        border: none;
        background: none;
        color: #2E2E2E;
        cursor: pointer;
    }

    .modifying-list-name{
        margin-top: 50px;
        padding-top: 8.3px;
        font-size: 24px;
        font-weight: 700;
        border: none;
        background: none;
        color: #2E2E2E;
        cursor: pointer;
        margin-bottom: 28px;    
    }

    .list-input {
        max-width: 90%;
        width: 65%;
        height: 62px;
        padding: 10px 10px;
        border: none;
        background-color: rgb(243, 240, 240);
        border-radius: 15px;
        margin-bottom: 80px;
        background-image: url("../assets/+.svg");
        background-position: 15px 20px;
        background-repeat: no-repeat;
    }

    .list-input:hover{
        border: 1px solid #c6c6c6;
    }
    
    .list-input:focus::placeholder {
        color: transparent;
    }

    .list-input:focus{
        outline: none !important;
        border: 1px solid #c6c6c6;
        background-image: none; /* Remove the background image */
        font-size: 20px;
    }


    .checkbox-round {
        width: 1.3em;
        height: 1.3em;
        background-color: white;
        border-radius: 50%;
        vertical-align: middle;
        border: 1px solid #b3b3b3;
        appearance: none;
        -webkit-appearance: none;
        outline: none;
        cursor: pointer;
        margin-right: 26px;
    }

    .checkbox-round:checked {
        background-color: #db6868;
        border: none;
    }

    .checkbox-round:checked::before {
        content: "\2713"; /* Unicode character for a checkmark symbol */
        display: inline-block;
        width: 100%;
        text-align: center;
        color: white;
    }

    .task-list {
        list-style: none;
        padding: 0;
    }

    .task-item {
        position: relative;
        display: flex;
        align-items: center;
        margin-bottom: 10px;
    }

    .task-item::after {
        margin-bottom: -55px;
        content: "";
        position: absolute;
        left: 0;
        bottom: -1px;
        width: 65%;
        height: 1px;
        background-color: #dddddd;
    }

    .remove-task {
        background: none;
        border: none;
        position: absolute;
        left: 63%;
        cursor: pointer;
    }

    .remove-task img{
        width: 13.80px;
        height: 13.80px;
    }

    .list ul {
        display: flex;
        flex-direction: column;
    }

    .list ul li {
        margin-bottom: 80px;
        font-size: 14px;
        font-weight: 500;
    }

    .checked {
        text-decoration: line-through;
    }
</style>
