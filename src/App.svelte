<script>
	let strength = 0;
	let validations = [];
	let showPassword = false;

	const validatePassword = (e) => {
		const password = e.target.value;

		validations = [
			(password.length > 5),
			(password.search(/[A-Z]/) > -1),
			(password.search(/[0-9]/) > -1),
			(password.search(/[$&+,:;=?@#]/) > -1)
		];

		strength = validations.reduce((acc, cur) => acc + cur, 0);
	}
</script>

<style>
	form {
		--text-color: #afafaf;
	}

	.form-field {
		width: 100%;
		position: relative;
		border-bottom: 2px dashed var(--text-color);
		margin: 4rem auto 1rem;
	}

	.form-label {
		color: var(--text-color);
		font-size: 1.2rem;
	}

	.form-input {
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

	.form-input:valid {
		color: yellowgreen;
	}

	.form-input:invalid {
		color: orangered;
	}

	.form-field::after {
		content: "";
		position: relative;
		display: block;
		height: 4px;
		width: 100%;
		background: #d16dff;
		transform: scaleX(0);
		transform-origin: 0%;
		transition: transform 500ms ease;
		top: 2px;
	}

	.form-field:focus-within {
		border-color: transparent;
	}

	.form-field:focus-within::after {
		transform: scaleX(1);
	}
	
	.form-label {
		z-index: -1;
		position: absolute;
		transform: translateY(-2rem);
		transform-origin: 0%;
		transition: transform 400ms;
	}

	.form-field:focus-within .form-label,
	.form-input:not(:placeholder-shown) + .form-label {
		transform: scale(0.8) translateY(-5rem);
	}

	.form-pw-strength {
		display: flex;
		height: 20px;
		width: 100%;
	}
	
	.bar {
		margin-right: 5px;
		height: 100%;
		width: 25%;
		transition: box-shadow 500ms;
		box-shadow: inset 0 20px #1f1f1f;
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

	ul {
		list-style: none;
		margin: 10px 0;
		padding: 0;
	}

	.toggle-password {
		position: absolute;
		cursor: help;
		font-size: 0.8rem;
		right: 0.25rem;
		bottom: 0.5rem;
	}
</style>

<main>
	<form>

		<div class="form-field">
			<input type="email" name="emaiL" class="form-input" placeholder=" "/>
			<label for="email" class="form-label">E-mail</label>
		</div>

		<div class="form-field">
			<input type="{showPassword ? 'text' : 'password'}" name="password" class="form-input" placeholder=" "
			on:input={validatePassword}/>
			<label for="password" class="form-label">Password</label>
			<span class="toggle-password" on:mouseenter={() => showPassword = true}
			on:mouseleave={() => showPassword = false}>{showPassword ? '🙈' : '👁️'}</span>
		</div>

		<div class="form-pw-strength">
			<span class="bar bar-1" class:bar-show={strength > 0}/>
			<span class="bar bar-2" class:bar-show={strength > 1}/>
			<span class="bar bar-3" class:bar-show={strength > 2}/>
			<span class="bar bar-4" class:bar-show={strength > 3}/>
		</div>

		<ul>
			<li>{validations[0] ? '✔️' : '❌'} must be at least 5 characters</li>
			<li>{validations[1] ? '✔️' : '❌'}must contain a capital letter</li>
			<li>{validations[2] ? '✔️' : '❌'}must contain a number</li>
			<li>{validations[3] ? '✔️' : '❌'}must contain on of $&+,:;=?@#</li>
		</ul>
	</form>
</main>