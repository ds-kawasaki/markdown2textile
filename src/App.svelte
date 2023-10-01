<script lang="ts">
	let inputMarkdown = '';
	let outputTextile = '';
	$: outputTextile = convert(inputMarkdown);

	function convert(orgString: string): string {
		let buffer: string = orgString;
		buffer = buffer.replace(/^#### /gm, 'h4. '); //  見出し変換 
		buffer = buffer.replace(/^### /gm, 'h3. ');
		buffer = buffer.replace(/^## /gm, 'h2. ');
		buffer = buffer.replace(/^# /gm, 'h1. ');
		buffer = buffer.replace(/(h[1-4]\. .*)(\r\n|\r|\n)(?!(\r\n|\r|\n))/g, '$1$2$2$3');  //  見出しの次の行に空行追加
		return buffer;
	}
	function clearInput() {
		inputMarkdown = "";
	}
	function copyToClipBoard() {
		const outArea = document.querySelector<HTMLTextAreaElement>('#outArea');
		outArea.select();
		document.execCommand('copy');
	}
</script>

<main>
	<div>
		<div class='contents'>
			<label for='inArea'>Markdownを入力:</label>
			<div class='btn'>
				<button on:click={clearInput}>クリア</button>
			</div>
		</div>
		<textarea bind:value={inputMarkdown} id='inArea'></textarea>
	</div>
	<div>
		<div class='contents'>
			<label for='outArea'>textile</label>
			<div class='btn'>
				<button on:click={copyToClipBoard}>クリップボードにコピー</button>
			</div>
		</div>
		<textarea value={outputTextile} id='outArea'></textarea>
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	textarea {
		width: 100%;
		height: 200px;
	}

	.contents {
		display: flex;
	}

	.btn {
		margin-left: auto;
		text-align: right;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>