<script>
import {browser} from '$app/environment'
let inp, projects, task, list;
if(browser && localStorage.getItem("data") == undefined){
    browser && localStorage.setItem("data", JSON.stringify([{name:"first list", deleted:false, tasks:[{name:"first task",id:Math.random(), finished:false, deleted:false, id:Math.random()}]}]))
}
projects = browser && localStorage.getItem("data")
if(projects != []){
projects = JSON.parse(projects)
}
console.log(projects)
function delete_task(task_id, belongsTo){
    for(let i=0; i<projects.length; i++){
    if(projects[i].name == belongsTo) {
        for (let l = 0; l < projects[i].tasks.length; l++) {
            if(projects[i].tasks[l].id == task_id){
                projects[i].tasks[l].deleted = true
            }
        }
    }
}
projects = projects
save()
}
function finish_task(task_id, belongsTo){
    for(let i=0; i<projects.length; i++){
    if(projects[i].name == belongsTo) {
        for (let l = 0; l < projects[i].tasks.length; l++) {
            if(projects[i].tasks[l].id == task_id){
                projects[i].tasks[l].done = !projects[i].tasks[l].done
            }
        }
    }
}
projects = projects
save()
}
function save(){
    if(projects != []){
    browser && localStorage.setItem("data", JSON.stringify(projects))
    }
}
function addtask(project_name, p){
    for(let i=0; i<projects.length; i++){
    if(projects[i].id == project_name) {
        projects[i].tasks.push({name:p, done:false, deleted:false, id:Math.random()})
    }
}
projects = projects 
save()
}
function addlist(){
    if(list == undefined || list == "" || list.replace(/\s/g, '') == '') return
    if(projects == ""){
    projects = []
}
    projects.push({name:list, tasks:[], id:Math.random(), text:""})
    projects = projects
    save()
}
function deletelist(list_id){
    for(let i=0; i<projects.length; i++){
    if(projects[i].id == list_id) {
        projects[i].deleted = true
    }
}
projects = projects
save()
}
</script>
<div class="body">
    <div class="addlist">
        <p class="in">add a list:</p><br><input placeholder="name" bind:value={list} class="in addinp">
        <button on:click={addlist} class="sub">submit</button>
    <br><br>
        <a href="/deleted">recover deleted</a>
    </div>
{#each projects as project}
{#if !project.deleted}
<div class="list">
<h2><i>{project.name}</i>: </h2>
add a task:<br><input placeholder="name" bind:value={project.text} class="add"><button on:click={() => addtask(project.id, project.text)} class="addbutton">add</button>
<br>
{#if project.tasks.length > 0}
    {#each project.tasks as task}
    {#if !task.deleted}
    {#if !task.done}
    <div class="task"><i class="taskname">{task.name}</i><button on:click={() => finish_task(task.id, project.name)} class="delete ss">✔️</button><button on:click={() => delete_task(task.id, project.name)} class="delete">🗑️</button></div>
    {:else}
    <div class="finished task"><i class="taskname">{task.name}</i><button on:click={() => finish_task(task.id, project.name)} class="delete ss">✔️</button><button on:click={() => delete_task(task.id, project.name)} class="delete">🗑️</button></div>
    {/if}
    {/if}
    {/each}
{:else}
<p>no tasks yet.</p>
{/if}
</div>
<div class="deletediv"><button on:click={() => deletelist(project.id)} style="background-color: white;" class="deletebutton">delete list 🗑️</button></div>
{/if}
{/each}
</div>
<style>
    .finished{
        text-decoration: line-through;
    }
    .task{
        margin-left: 4vw;
        margin-top: 0.5vh;
        font-size: 20px;
        border: solid 2px black;
        width: 70vw;
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
        font-size: 15px;
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
.add{
    background-color: transparent;
    border: solid 1px black;
    color: black;
}
.addbutton{
    background-color: white;
}
.sub{
    background-color: white;
    margin-left: -3.75vw;
    height: 3.5vh;
}
.addlist{
    text-align: center;
    font-size: 1.5vw;
}
.list{
    overflow-x: hidden;
    text-align: center;
    border: solid 1px black;
    width: 80vw;
    margin-left: 10vw;
    background-color: white;
    margin-top: 2vh;
    overflow-y: scroll;
    max-height: 25vh;
    scroll-behavior: smooth;
}
.in{
    display: inline;
}
.addinp{
height: 3vh;
width: 20vw;
}
.ss{
    margin-right: 4vw;
}
.deletediv{
    margin-left: 10vw;
}
.deletebutton{
    width: 80.2vmin;
}
</style>