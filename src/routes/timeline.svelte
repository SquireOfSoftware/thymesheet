<script context="module">
	import { browser, dev } from '$app/env';

	export const hydrate = dev;

	export const router = browser;

	export const prerender = true;
</script>

<svelte:head>
	<title>Timelines</title>
	<meta name="description" content="Timelining the app" />
</svelte:head>

<script>
	// import { LOGNAME } from '$env/static/private';
import {
		Timeline,
		TimelineItem,
		TimelineSeparator,
		TimelineDot,
		TimelineConnector,
		TimelineContent,
        TimelineOppositeContent
	} from 'svelte-vertical-timeline';

    let tasks = [{name: 'TIME-1'}, 
            {name: 'TIME-2'}, 
            {name: 'TIME-3'}];

    let selectedTask = tasks[0];
    let hours = 0;

	let options = [{ title: 'Eat', time: 2 }, 
            { title: 'Sleep', time: 8 }, 
            { title: 'Code', time: 8 }];

    function addOption () {
        options.push({ title: selectedTask.name, time: hours });
        console.log({selectedTask});
        options = options;
    }

    function removeTask(option) {
        console.log('removing ${option}')
        let position = options.indexOf(option);
        options.splice(position, 1);
        options = options;
    }
</script>


<div id="timeline_body">
    <p>This is a timeline app</p>
    
    <div>
        <form on:submit|preventDefault={addOption }>
            <select on:change={event => {
                console.log({item: event.target?.value, selectedTask});
                return selectedTask = {name: event.target?.value};
            }}
            >
                {#each tasks as task}
                    <option value={task.name}>
                        {task.name}
                    </option>
                {/each}
            </select>

            <input type="number" bind:value={hours}>
        
            <button  type=submit>
                Submit
            </button>
        </form>
    </div>
    <Timeline>
        {#each options as option}
            <TimelineItem>
                <TimelineSeparator>
                    <TimelineDot />
                    <TimelineConnector />
                </TimelineSeparator>
                <TimelineContent>
                    <p>Hours: {option.time}</p>
                </TimelineContent>
                <TimelineOppositeContent slot="opposite-content">
                    <h3>Task: {option.title}</h3>
                </TimelineOppositeContent>
                <div on:click={() => removeTask(option)}>
                    Remove
                </div>
            </TimelineItem>
        {/each}
    </Timeline>
</div>

<style>
    #timeline_body {
        /* height: 100rem; */
    }
</style>