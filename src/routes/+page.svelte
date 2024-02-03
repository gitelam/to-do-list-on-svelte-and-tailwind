
<script>
    
    // @ts-ignore
    import { json } from "@sveltejs/kit";
    import "../app.css";
    import Icon from '@iconify/svelte';
    import { onMount } from "svelte";

    

    // @ts-ignore
    /**
   * @type {any[]}
   */
    let tasksToDo = [];
    let taskName = "";
    let numTask = 0;
    let isEmpty = false;
    const onlySpaces = new RegExp("^[ ]+$");

    onMount(() => {

        if (localStorage.getItem("array") === null || localStorage.getItem("numtask") === null){
            
            localStorage.setItem("array", JSON.stringify([]));
            localStorage.setItem("numtask", "0");

        } else {
            
            // @ts-ignore
            tasksToDo = JSON.parse(localStorage.getItem("array"));
            // @ts-ignore
            numTask = parseInt(localStorage.getItem("numtask"));

        }

    });


    const addItem = () => {

    // @ts-ignore
    if (taskName === "" || onlySpaces.test(taskName) === true){
      isEmpty = true;
      return;
    }
    
    numTask = numTask + 1;
    
    tasksToDo = [
      // @ts-ignore
      ...tasksToDo,
      {
        numTask: numTask,
        // @ts-ignore
        desc: taskName,
        done: false
      }
    ];


    loadTasks();
    debugTasks();

    // @ts-ignore
    taskName = "";

  };

  const remove = (/** @type {{ numTask: number, desc: string; done: boolean; }} */ task) => {
    // @ts-ignore
    tasksToDo = tasksToDo.filter(i => i !== task);
    
    loadTasks();
    debugTasks();

  };

  // @ts-ignore
  const toggle = (/** @type {{ done: boolean; }} */ task) => {
    task.done = !task.done;

    loadTasks();
    debugTasks();

    // @ts-ignore
    tasksToDo = tasksToDo;
  };

  const debugTasks = () => {
    console.log(localStorage.getItem("array"));
  };

  const loadTasks = () => {

    // @ts-ignore
    localStorage.setItem("array", JSON.stringify(tasksToDo));
    
    localStorage.setItem("numtask", numTask.toString());

  };

  const Writing = () => {
    isEmpty = false;
  };


</script>

<style>
  .warning{
    color: rgb(235, 80, 80);
  }

</style>



<body class="p-20 bg-slate-900">
   
        <div class="w-auto max-w-96 m-auto p-2 border-solid rounded-3xl bg-slate-100">
            <div class="items m-4">

                <div class="head mb-2 border-black border-b-2">
                                        

                  <div class="flex items-center">
                    <h1 class="w-full text-2xl text-left mb-1">The to-do list</h1>
                    
                    {#if isEmpty}
                      <div id="noInput" class="flex w-full justify-end">
                        <div class="warning mr-1">
                          <Icon icon="ic:round-warning-amber" width="24"/>
                        </div>
                        <p class="text-red-500 font-medium">the field is empty</p>
                      </div>
                    {/if}
                  
                  </div>

                <form on:submit|preventDefault={addItem}>
                    
                    <div class="flex mt-2 mb-2 items-center border-1 rounded-xl border-slate-200">
                        <input type="text" bind:value={taskName} on:input={Writing} class="w-full rounded-l-lg p-2 bg-slate-200" placeholder="write a task">
                        
                        <button type="submit" id="add-button" class="flex w-fit p-2 min-w-14 items-center justify-center bg-slate-300 hover:bg-slate-500 hover:text-slate-100 rounded-r-lg">
                            add
                        </button>
                    </div>

                </form>
               
                    
                </div>

                <div class="overflow-auto max-h-60">
                    <ul>
                        
                        {#each tasksToDo as task}
                        <li class:done={task.done}>
                            <div class="flex items-center mb-2 bg-slate-300 rounded-xl">

                                {#if task.done === true}
                                 <input on:change={()=>toggle(task)} checked id="default-checkbox{task.numTask}" type="checkbox" class="ml-2 w-4 h-4 bg-gray-100 border-gray-300 rounded  dark:bg-gray-700 dark:border-gray-600">
                                {:else}
                                 <input on:change={()=>toggle(task)} id="default-checkbox{task.numTask}" type="checkbox" class="ml-2 w-4 h-4 bg-gray-100 border-gray-300 rounded  dark:bg-gray-700 dark:border-gray-600">

                                {/if}
                                
                                 <label for="default-checkbox{task.numTask}" class="w-full ml-2 p-2">{task.desc}</label>
                                 <button on:click={() => remove(task)} id="delete-button" class="flex items-center m-2 justify-end mr-4 hover:bg-slate-100 rounded-lg p-0">
                                    <label for="delete-buttton"></label>
                                    <Icon icon="material-symbols:delete-sweep-outline" width="24"/>
                                </button>
                            </div>
                        </li>
                        {/each}
                        
                    </ul>
                </div>
                
            </div>
        </div>

</body>


