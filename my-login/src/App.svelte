<script>
	// JS here..
	let strength = 0;
	let validations = [];
	let showPassword = false

	function validatePassword(e) {
		const password = e.target.value;

		validations = [
			(password.length > 8),
			// regex to search for patterns in text ⤵ 
			(password.search(/[A-Z]/) > -1),
			(password.search(/[0-9]/) > -1),
			(password.search(/[$&+,;:=?@#]/) > -1)
		]

		strength = validations.reduce((acc, cur) => acc + cur);
	}
</script>

<style>
	/* CSS here... */
	form {
		/* css variable for easy theme changes */
		--text-color: #afafaf;
		max-width: 500px;
	}
	/* container for the input and label */
	.field {
		width: 100%;
		position: relative;
		border-bottom: 2px dashed var(--text-color);
		margin: 4rem auto 1rem;
	}

	.label {
		color: var(--text-color);
		font-size: 1.2rem;
	}

	.input {
		outline: none;
		border: none;
		overflow: hidden;
		margin: 0;
		width: 100%;
		padding: 0.25rem 0;
		background: none;
		color: white;
		font-size: 1.2rem;
		font-weight: bold;
	}
	/* pseudo selectors */
	.input:valid {
		color: yellowgreen;
	}

	.input:invalid {
		color: orangered;
	}
	/* border animation with pseudo element */
	.field::after {
		content: '';
		position: relative;
		display: block;
		height: 4px;
		width: 100%;
		background: purple;
		transform: scaleX(0);
		transform-origin: 0%;
		transition: transform 500ms ease;
		top: 2px;
	}
	/* state change, active when child has focus */
	.field:focus-within {
		border-color: transparent;
	}

	.field:focus-within::after {
		transform: scaleX(1);
	}
	/* label animation, label comes after input in html */
	.label {
		z-index: -1;
		position: absolute;
		transform: translateY(-2rem);
		transform-origin: 0%;
		transition: transform 400ms;
	}

	.field:focus-within .label,
	.input:not(:placeholder-shown) + .label {
		transform: scale(0.8) translateY(-5rem);
	}
	/* strength meter */
	.strength {
		display: flex;
		height: 20px;
		width: 100;
	}

	.bar {
		margin-right: 5px;
		height: 100%;
		width: 25%;
		transition: box-shadow 500ms;
		box-shadow: inset 0px 20px #1f1f1f;
	}

	.bar-show {
		box-shadow: none;
	}

	.bar-1 {
		background: linear-gradient(to right, red, orangered);
	}

	.bar-2 {
		background: linear-gradient(to right, orangered, yellow);
	}

	.bar-3 {
		background: linear-gradient(to right, yellow, yellowgreen);
	}

	.bar-4 {
		background: linear-gradient(to right, yellowgreen, green);
	}

	
	

</style>

<!-- HTML here.. -->
<main>
	<form>
		<div class="field">
			<input type="email" name="email" class="input" placeholder="" />
			<label for="email" class="label">Email</label>
		</div>
		<div class="field">
			<input type="password" class="input" placeholder="" on:input={validatePassword} />
			<label for="password" class="label">Password</label>
		</div>
		<div class="strength">
			<span class="bar bar-1" class:bar-show={strength > 0}/>
			<span class="bar bar-2" class:bar-show={strength > 1}/>
			<span class="bar bar-3" class:bar-show={strength > 2}/>
			<span class="bar bar-4" class:bar-show={strength > 3}/>
		</div>

		<ul>
			<li> {validations[0] ? ':heavy_check_mark:' : ':heavy_multiplication_x:'} must be at least 8 characters</li>
			<li> {validations[1] ? ':heavy_check_mark:' : ':heavy_multiplication_x:'} must contain a capital letter</li>
			<li> {validations[2] ? ':heavy_check_mark:' : ':heavy_multiplication_x:'} must contain a number</li>
			<li> {validations[3] ? ':heavy_check_mark:' : ':heavy_multiplication_x:'} must contain one of $&+,;:=?@#</li>
		</ul>
	</form>
</main>