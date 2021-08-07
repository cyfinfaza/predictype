<script>
	let userText = "";
	let lastKnownText = userText;
	let predictedText = "";
	let predicting = false;
	let userSpan;
	// $:{
	// 	if(userText == ""){
	// 		predictedText = "";
	// 	} else {
	// 		fetch('http://localhost:5004/predict', {method: 'POST', body: userText}).then(response => response.text()).then(responseText => predictedText=responseText);
	// 	}
	// }
	setInterval(async () => {
		if(userText == ""){
			predictedText = "";
		} else if (lastKnownText != userText && !predicting) {
			predicting = true;
			await fetch('http://localhost:5004/predict', {method: 'POST', body: userText}).then(response => response.text()).then(responseText => predictedText=responseText);
			predicting = false;
			lastKnownText = userText;
		}
	}, 100);
	function setCaretToEnd(target) {
		const range = document.createRange();
		const sel = window.getSelection();
		range.selectNodeContents(target);
		// range.collapse(false);
		// sel.removeAllRanges();
		// sel.addRange(range);
		// target.focus();
		// range.detach(); // optimization

		// // set scroll to the end if multiline
		// target.scrollTop = target.scrollHeight; 
	}
	function keyPressed(e){
		console.log(e)
		if(e.key == "Tab"){
			e.preventDefault()
			userText += predictedText
			setCaretToEnd(userSpan)
			// userSpan.setSelectionRange(userSpan.innerText.length, userSpan.innerText.length.length);
		}
	}
</script>

<main>
	<pre><span id="userSpan" contenteditable="" bind:this={userSpan} autofocus bind:textContent={userText} on:keydown={keyPressed}></span><span id="predictedSpan">{predictedText}</span></pre>
</main>

<style>
	main {
		/* text-align: center; */
		padding: 1em;
		/* max-width: 240px; */
		margin: 0 auto;
	}

	#predictedSpan{
		color: #8888;
	}

	#userSpan {
		border: none;
		display: inline-block;
	}

	#userSpan:focus {
		outline: none;
	}

	#userSpan:empty::before {
		content: "Start typing here";
		color: #8888;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>