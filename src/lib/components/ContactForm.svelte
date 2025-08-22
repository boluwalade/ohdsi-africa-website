<script lang="ts">
	let isSubmitting = false;
	let isSubmitted = false;
	let error = '';

	async function handleSubmit(event: Event) {
		event.preventDefault();
		isSubmitting = true;
		error = '';

		const form = event.target as HTMLFormElement;
		const formData = new FormData(form);

		try {
			const response = await fetch('/', {
				method: 'POST',
				headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
				body: new URLSearchParams(formData as any).toString()
			});

			if (response.ok) {
				isSubmitted = true;
				form.reset();
			} else {
				throw new Error('Form submission failed');
			}
		} catch (err) {
			error = 'There was an error sending your message. Please try again.';
		} finally {
			isSubmitting = false;
		}
	}

	function resetForm() {
		isSubmitted = false;
		error = '';
	}
</script>

<h2>Send us a Message</h2>

{#if isSubmitted}
	<div class="success-message">
		<i class="fas fa-check-circle"></i>
		<h3>Message Sent Successfully!</h3>
		<p>Thank you for your message. We will get back to you soon.</p>
		<button on:click={resetForm} class="btn btn-secondary">Send Another Message</button>
	</div>
{:else}
	<form 
		on:submit={handleSubmit} 
		class="contact-form"
		name="contact"
		method="POST"
		data-netlify="true"
		netlify-honeypot="bot-field"
	>
		<!-- Hidden fields for Netlify -->
		<input type="hidden" name="form-name" value="contact" />
		<div style="display: none;">
			<label>
				Don't fill this out if you're human: 
				<input name="bot-field" />
			</label>
		</div>

		{#if error}
			<div class="error-message">
				<i class="fas fa-exclamation-triangle"></i>
				{error}
			</div>
		{/if}

		<div class="form-group">
			<label for="name">Name *</label>
			<input 
				type="text" 
				id="name" 
				name="name"
				required 
				placeholder="Your full name"
				disabled={isSubmitting}
			/>
		</div>

		<div class="form-group">
			<label for="email">Email *</label>
			<input 
				type="email" 
				id="email" 
				name="email"
				required 
				placeholder="your.email@example.com"
				disabled={isSubmitting}
			/>
		</div>

		<div class="form-group">
			<label for="subject">Subject *</label>
			<input 
				type="text" 
				id="subject" 
				name="subject"
				required 
				placeholder="What is your message about?"
				disabled={isSubmitting}
			/>
		</div>

		<div class="form-group">
			<label for="message">Message *</label>
			<textarea 
				id="message" 
				name="message"
				required 
				placeholder="Your message to OHDSI Africa..."
				rows="6"
				disabled={isSubmitting}
			></textarea>
		</div>

		<button type="submit" class="submit-btn" disabled={isSubmitting}>
			{#if isSubmitting}
				<i class="fas fa-spinner fa-spin"></i>
				Sending...
			{:else}
				<i class="fas fa-paper-plane"></i>
				Send Email
			{/if}
		</button>
	</form>
{/if}

<style>
	.contact-form {
		max-width: 600px;
		margin: 0 auto;
	}

	.form-group {
		margin-bottom: var(--spacing-lg);
	}

	label {
		display: block;
		margin-bottom: var(--spacing-sm);
		font-weight: 600;
		color: var(--primary-blue);
	}

	input,
	textarea {
		width: 100%;
		padding: var(--spacing-md);
		border: 2px solid var(--light-gray);
		border-radius: var(--radius-md);
		font-size: var(--font-size-base);
		font-family: var(--font-family);
		transition: border-color 0.3s ease;
	}

	input:focus,
	textarea:focus {
		outline: none;
		border-color: var(--secondary-blue);
		box-shadow: 0 0 0 3px rgba(59, 130, 246, 0.1);
	}

	input:disabled,
	textarea:disabled {
		background-color: var(--light-gray);
		opacity: 0.7;
		cursor: not-allowed;
	}

	textarea {
		resize: vertical;
		min-height: 120px;
	}

	.submit-btn {
		background: linear-gradient(135deg, var(--primary-blue), var(--secondary-blue));
		color: var(--white);
		border: none;
		padding: var(--spacing-md) var(--spacing-xl);
		border-radius: var(--radius-lg);
		font-size: var(--font-size-lg);
		font-weight: 600;
		cursor: pointer;
		transition: all 0.3s ease;
		display: flex;
		align-items: center;
		gap: var(--spacing-sm);
		margin: var(--spacing-xl) auto 0;
	}

	.submit-btn:hover:not(:disabled) {
		transform: translateY(-2px);
		box-shadow: var(--shadow-lg);
	}

	.submit-btn:active {
		transform: translateY(0);
	}

	.submit-btn:disabled {
		opacity: 0.7;
		cursor: not-allowed;
		transform: none;
	}

	.success-message {
		text-align: center;
		padding: var(--spacing-2xl);
		background: linear-gradient(135deg, var(--light-blue), var(--white));
		border-radius: var(--radius-lg);
		border: 2px solid var(--success);
	}

	.success-message i {
		font-size: var(--font-size-3xl);
		color: var(--success);
		margin-bottom: var(--spacing-lg);
	}

	.success-message h3 {
		color: var(--primary-blue);
		margin-bottom: var(--spacing-md);
	}

	.success-message p {
		margin-bottom: var(--spacing-xl);
		color: var(--text-dark);
	}

	.error-message {
		background-color: #fef2f2;
		border: 1px solid var(--error);
		color: var(--error);
		padding: var(--spacing-md);
		border-radius: var(--radius-md);
		margin-bottom: var(--spacing-lg);
		display: flex;
		align-items: center;
		gap: var(--spacing-sm);
	}

	.btn {
		display: inline-flex;
		align-items: center;
		gap: var(--spacing-sm);
		padding: var(--spacing-md) var(--spacing-xl);
		border-radius: var(--radius-lg);
		text-decoration: none;
		font-weight: 600;
		font-size: var(--font-size-base);
		transition: all 0.3s ease;
		border: 2px solid transparent;
		cursor: pointer;
		background: none;
	}

	.btn-secondary {
		background-color: transparent;
		color: var(--primary-blue);
		border-color: var(--primary-blue);
	}

	.btn-secondary:hover {
		background-color: var(--primary-blue);
		color: var(--white);
		transform: translateY(-2px);
		box-shadow: var(--shadow-lg);
	}

	@media (max-width: 768px) {
		.contact-form {
			max-width: 100%;
		}
		
		.submit-btn {
			width: 100%;
			justify-content: center;
		}
	}
</style>