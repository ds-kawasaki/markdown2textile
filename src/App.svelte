<script lang="ts">
	let inputMarkdown: string;
	let outputTextile: string;

	const convertTextile = () => {
		var convert = (orgString: string): string => {
			let buffer = orgString;
			buffer = buffer.replace(/^### /gm, 'h3. '); //  見出し変換 
			buffer = buffer.replace(/^## /gm, 'h2. ');
			buffer = buffer.replace(/^# /gm, 'h1. ');
			buffer = buffer.replace(/(h[1-3]\. .*)(\r\n|\r|\n)(?!(\r\n|\r|\n))/g, '$1$2$2$3');  //  見出しの次の行に空行追加
			return buffer;
		};

		outputTextile = convert(inputMarkdown);
		const outTextarea: HTMLTextAreaElement = <HTMLTextAreaElement>document.getElementById('outArea');
		outTextarea.value = outputTextile;
	};
</script>

<main>
	<div>
		<label for='inArea'>Markdownを入力:</label>
		<textarea bind:value={inputMarkdown} id='inArea'></textarea>
	</div>
	<div>
		<button on:click={convertTextile}>
			Textileに変換
		</button>
	</div>
	<div>
		<textarea id='outArea'></textarea>
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

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>