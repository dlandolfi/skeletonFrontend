<script lang="ts">
	let conn: WebSocket;
	var msg = document.getElementById('msg');
	var log = document.getElementById('log');

	import { onMount } from 'svelte';

	function appendLog(item: HTMLDivElement) {
		if (log) {
			const doScroll = log.scrollTop > log.scrollHeight - log.clientHeight - 1;
			log.appendChild(item);
			if (doScroll) {
				log.scrollTop = log.scrollHeight - log.clientHeight;
			}
		}
	}

	const handleSubmit = () => {
		if (!conn) {
			return false;
		}
		if (!msg) {
			return false;
		}
		console.log('msg', msg);
		conn.send(msg);
		msg = null;
		return false;
	};

	onMount(() => {
		if (window.WebSocket) {
			conn = new WebSocket(`ws://localhost:8080/ws`);
			conn.onclose = (evt) => {
				const item = document.createElement('div');
				item.innerHTML = '<b>Connection closed.</b>';
				appendLog(item);
			};
			conn.onmessage = (evt) => {
				const messages = evt.data.split('\n');
				messages.forEach((message: string) => {
					const item = document.createElement('div');
					item.innerText = message;
					appendLog(item);
				});
			};
		} else {
			const item = document.createElement('div');
			item.innerHTML = '<b>Your browser does not support WebSockets.</b>';
			appendLog(item);
		}
	});
</script>

<div id="log" class="m-8 p-8 bg-white"></div>
<form id="form" class="m-8" on:submit={handleSubmit}>
	<input type="submit" value="Send" />
	<input type="text" bind:value={msg} size="64" autofocus />
</form>

<style>
	/* #log {
		background: white;
		margin: 0;
		padding: 0.5em 0.5em 0.5em 0.5em;
		position: absolute;
		top: 0.5em;
		left: 0.5em;
		right: 0.5em;
		bottom: 3em;
		overflow: auto;
	} */

	/* #form {
		padding: 0 0.5em 0 0.5em;
		margin: 0;
		position: absolute;
		bottom: 1em;
		left: 0px;
		width: 100%;
		overflow: hidden;
	} */
</style>
