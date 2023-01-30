<script>
import {browser} from '$app/environment'
let  projects;
if(browser && localStorage.getItem("data") == undefined){

}else{
projects = browser && localStorage.getItem("data")
projects = JSON.parse(projects)
}
function save(){
    browser && localStorage.setItem("data", JSON.stringify(projects))
}
function recover_task(task_id, belongsTo){
    for(let i=0; i<projects.length; i++){
    if(projects[i].id == belongsTo) {
        for (let l = 0; l < projects[i].tasks.length; l++) {
            if(projects[i].tasks[l].id == task_id){
                projects[i].tasks[l].deleted = false
            }
        }
    }
    }
    projects = projects
    save()
}
function recover_list(id){
for (let i = 0; i < projects.length; i++) {
   if(projects[i].id == id){
    projects[i].deleted = false
    for (let l = 0; l < projects[i].tasks.length; l++) {
    projects[i].tasks[l].deleted = false
    }
   }
}
projects = projects
    save()
}
</script>
<a href="./">Back</a><br>
<h2>Deleted lists: </h2><br>
{#each projects as project}
{#if project.deleted}
<div class="list">
<h2><i>{project.name}: </i></h2><br>
{#each project.tasks as task}
{#if !task.done}
    <div class="task"><i class="taskname">{task.name}</i></div><br>
    {:else}
    <div class="finished task"><i class="taskname">{task.name}</i></div><br>
{/if}
{/each}
<button on:click={() => recover_list(project.id)}>recover list</button>
</div>
{/if}
{/each}
<br>
<h2>Deleted tasks:</h2>
<br>
{#each projects as project}
{#if !project.deleted}
{#each project.tasks as task}
{#if task.deleted}
{#if task.finished}
<div class="finished task"><i class="taskname">{task.name} ({project.name}) (finished)</i><button on:click={() => recover_task(task.id, project.id)} class="delete">recover</button>
</div>
{:else}
<div class="task"><i class="taskname">{task.name} ({project.name})</i><button on:click={() => recover_task(task.id, project.id)} class="delete">recover</button>
</div>
{/if}
{/if}
{/each}
{/if}
{/each}
<style>
    .finished{
        text-decoration: line-through;
    }
    .task{
        margin-left: 2vw;
        font-size: 20px;
        border: solid 2px black;
        width: 10vw;
        background: rgb(131,58,180);
        background: linear-gradient(-45deg, rgba(131,58,180,1) 0%, rgba(253,29,29,1) 50%, rgba(252,176,69,1) 100%);
        -webkit-animation: gradient 20s ease infinite;
	        animation: gradient 20s ease infinite;
        background-size: 300% 300%;
        position:relative;
        word-wrap: break-word;
    }
    h2{
        font-size: 30px;
    }
    :global(body){
        animation: col 5s infinite;
        background: linear-gradient(-45deg, red, #ee7752, #e73c7e, #23a6d5, #23d5ab);
	background-size: 400% 400%;
	-webkit-animation: gradient 20s ease infinite;
	        animation: gradient 20s ease infinite;
	height: 100vh;
        width: 95vw;
        height: 100vh;
    }
    .delete{
        border: none;
        position: absolute;
        right:    0;
        bottom:   0;
    }
    button{ background-color: transparent;border: solid 0.5px black; }
    @-webkit-keyframes gradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}
 
@keyframes gradient {
	0% {
		background-position: 0% 50%;
	}
	50% {
		background-position: 100% 50%;
	}
	100% {
		background-position: 0% 50%;
	}
}
a{
    font-size: 20px;
    text-decoration: none;
    border: solid 1px black;
    background: linear-gradient(-45deg, lightblue, yellow, lightgreen );
    color: black;
    background-size: 300% 300%;
    -webkit-animation: gradient 10s ease infinite;
	        animation: gradient 10s ease infinite;
    display: flex;
    align-self: center;
    text-align: center;
    width: fit-content;
    margin-left: 43vw;
}

</style>