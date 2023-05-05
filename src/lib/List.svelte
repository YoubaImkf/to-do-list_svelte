<script>
    //Variable pour le nouvel item de la liste
    let newItem = "";

    //Array pour les items de la liste
    let list = [
        {id: 1, name: "Create a todo List", completed : false},
        {id: 2, name: "Add item to the List", completed : false},
        {id: 3, name: "Check items from the List", completed : false},
        {id: 4, name: "Remove items from the List", completed : false},
        {id: 5, name: "Add a new List", completed : false},
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

    //Fonction pour supprimer un item de la liste
    function removeFromList(index){
        list = list.filter((item, i) => i !== index);
    }

</script>
<div>
    <div></div>

    <h1>List 1</h1>
    <!-- Input pour ajouter un item à la liste -->
    <input type="text" bind:value={newItem} placeholder="new todo item">
    <button on:click={addToList}>Add</button>

    <!--for each pour ajouter chaque item à la liste-->
    <ul>
    {#each list as item, index}
      <li class:checked={item.completed}>
        <input type="checkbox" bind:checked={item.completed}/> {item.name} <button on:click={() => removeFromList(index)}>x</button>
      </li>
    {:else}
      Nothing to do here!
    {/each}
    </ul>
</div>

<style>
    .checked{
        text-decoration: line-through;
    }

    li{
        list-style: none;
    }
</style>