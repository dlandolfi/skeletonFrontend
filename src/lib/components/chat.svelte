<script lang="ts">
	let conn: WebSocket;
	let messages: any[] = [];
	let msg = '';

	function appendLog(item: { text: string }) {
		messages = [...messages, item];
		const log = document.getElementById('log');
		if (log) {
			log.scrollTop = log.scrollHeight;
		}
	}

	function handleSubmit(event: { preventDefault: () => void }) {
		event.preventDefault();
		if (!conn) return;
		if (!msg.trim()) return;
		conn.send(msg.trim());
		msg = '';
	}

	function initWebSocket() {
		if ('WebSocket' in window) {
			conn = new WebSocket('wss://danothebard.com/api/ws');
			// local testing
			// conn = new WebSocket('ws://' + document.location.hostname + ':8080' + '/api/ws');
			conn.onclose = () => {
				appendLog({ text: 'Connection closed.' });
			};
			conn.onmessage = (evt) => {
				const newMessages = evt.data.split('\n').map((text: any) => ({ text }));
				newMessages.forEach(appendLog);
			};
		} else {
			appendLog({ text: 'Your browser does not support WebSockets.' });
		}
	}

	$: initWebSocket(); // Initialize WebSocket on component mount
</script>

<div id="log" class="p-4 m-4 bg-white h-80 text-primary-500 overflow-auto">
	{#each messages as message}
		<div>{message.text}</div>
	{/each}
</div>

<form id="form" class="mx-4 text-secondary-500" on:submit={handleSubmit}>
	<button type="submit" class="btn variant-filled-surface">Send</button>
	<input type="text" class="ml-4" bind:value={msg} />
</form>

<style>
	/* #log {
		background: white;
		margin: 0;
		padding: 0.5em;
		position: absolute;
		top: 5em;
		left: 0.5em;
		right: 0.5em;
		bottom: 3em;
		overflow: auto;
	}

	#form {
		padding: 0 0.5em;
		margin: 0;
		position: absolute;
		bottom: 1em;
		left: 0;
		width: 100%;
	} */
</style>
