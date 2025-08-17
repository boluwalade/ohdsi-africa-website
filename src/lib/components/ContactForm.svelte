<script lang="ts">
	let formData = {
		firstName: '',
		lastName: '',
		email: '',
		organization: '',
		country: '',
		subject: '',
		message: '',
		newsletter: false,
		privacy: false
	};

	function handleSubmit(event: Event) {
		event.preventDefault();
		
		// Basic validation
		if (!formData.firstName || !formData.lastName || !formData.email || !formData.message || !formData.subject) {
			alert('Please fill in all required fields.');
			return;
		}
		
		if (!formData.privacy) {
			alert('Please agree to the privacy policy.');
			return;
		}
		
		// Simulate form submission
		alert('Thank you for your message! We will get back to you soon.');
		
		// Reset form
		formData = {
			firstName: '',
			lastName: '',
			email: '',
			organization: '',
			country: '',
			subject: '',
			message: '',
			newsletter: false,
			privacy: false
		};
	}
</script>

<h2>Send us a Message</h2>
<form on:submit={handleSubmit} class="contact-form">
	<div class="form-row">
		<div class="form-group">
			<label for="firstName">First Name *</label>
			<input type="text" id="firstName" bind:value={formData.firstName} required>
		</div>
		<div class="form-group">
			<label for="lastName">Last Name *</label>
			<input type="text" id="lastName" bind:value={formData.lastName} required>
		</div>
	</div>
	
	<div class="form-group">
		<label for="email">Email Address *</label>
		<input type="email" id="email" bind:value={formData.email} required>
	</div>
	
	<div class="form-group">
		<label for="organization">Organization/Institution</label>
		<input type="text" id="organization" bind:value={formData.organization} placeholder="University, Hospital, Research Institute, etc.">
	</div>
	
	<div class="form-group">
		<label for="country">Country</label>
		<select id="country" bind:value={formData.country}>
			<option value="">Select your country</option>
			<option value="Algeria">Algeria</option>
			<option value="Angola">Angola</option>
			<option value="Benin">Benin</option>
			<option value="Botswana">Botswana</option>
			<option value="Burkina Faso">Burkina Faso</option>
			<option value="Cameroon">Cameroon</option>
			<option value="Egypt">Egypt</option>
			<option value="Ethiopia">Ethiopia</option>
			<option value="Ghana">Ghana</option>
			<option value="Kenya">Kenya</option>
			<option value="Morocco">Morocco</option>
			<option value="Nigeria">Nigeria</option>
			<option value="Rwanda">Rwanda</option>
			<option value="Senegal">Senegal</option>
			<option value="South Africa">South Africa</option>
			<option value="Tanzania">Tanzania</option>
			<option value="Tunisia">Tunisia</option>
			<option value="Uganda">Uganda</option>
			<option value="Zambia">Zambia</option>
			<option value="Zimbabwe">Zimbabwe</option>
			<option value="Other">Other</option>
		</select>
	</div>
	
	<div class="form-group">
		<label for="subject">Subject *</label>
		<select id="subject" bind:value={formData.subject} required>
			<option value="">Select a topic</option>
			<option value="General Inquiry">General Inquiry</option>
			<option value="Joining OHDSI Africa">Joining OHDSI Africa</option>
			<option value="Technical Support">Technical Support</option>
			<option value="OMOP CDM Implementation">OMOP CDM Implementation</option>
			<option value="Training & Education">Training & Education</option>
			<option value="Research Collaboration">Research Collaboration</option>
			<option value="Partnership Opportunities">Partnership Opportunities</option>
			<option value="Funding Information">Funding Information</option>
			<option value="Event Inquiries">Event Inquiries</option>
			<option value="Other">Other</option>
		</select>
	</div>
	
	<div class="form-group">
		<label for="message">Message *</label>
		<textarea id="message" bind:value={formData.message} rows="6" required placeholder="Please provide details about your inquiry or how we can help you..."></textarea>
	</div>
	
	<div class="form-group checkbox-group">
		<label class="checkbox-label">
			<input type="checkbox" bind:checked={formData.newsletter}>
			I would like to receive updates about OHDSI Africa activities and events
		</label>
	</div>
	
	<div class="form-group checkbox-group">
		<label class="checkbox-label">
			<input type="checkbox" bind:checked={formData.privacy} required>
			I agree to the <a href="#" target="_blank" rel="noopener noreferrer">Privacy Policy</a> and consent to my data being processed for the purpose of this inquiry *
		</label>
	</div>
	
	<button type="submit" class="btn btn-primary">
		<i class="fas fa-paper-plane"></i> Send Message
	</button>
</form>

<style>
	h2 {
		font-size: var(--font-size-xl);
		font-weight: 600;
		color: var(--primary-blue);
		margin-bottom: var(--spacing-xl);
	}

	.contact-form {
		display: flex;
		flex-direction: column;
		gap: var(--spacing-lg);
	}

	.form-row {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--spacing-lg);
	}

	.form-group {
		display: flex;
		flex-direction: column;
		gap: var(--spacing-sm);
	}

	.form-group label {
		font-weight: 500;
		color: var(--text-dark);
	}

	.form-group input,
	.form-group select,
	.form-group textarea {
		padding: var(--spacing-md);
		border: 2px solid var(--light-blue);
		border-radius: var(--radius-md);
		font-family: var(--font-family);
		font-size: var(--font-size-base);
		transition: border-color 0.3s ease;
	}

	.form-group input:focus,
	.form-group select:focus,
	.form-group textarea:focus {
		outline: none;
		border-color: var(--secondary-blue);
	}

	.form-group textarea {
		resize: vertical;
		min-height: 120px;
	}

	.checkbox-group {
		flex-direction: row;
		align-items: flex-start;
		gap: var(--spacing-sm);
	}

	.checkbox-label {
		display: flex;
		align-items: flex-start;
		gap: var(--spacing-sm);
		cursor: pointer;
		font-size: var(--font-size-sm);
		line-height: 1.5;
	}

	.checkbox-label input[type="checkbox"] {
		margin: 0;
		width: 18px;
		height: 18px;
		accent-color: var(--primary-blue);
	}

	.checkbox-label a {
		color: var(--primary-blue);
		text-decoration: none;
	}

	.checkbox-label a:hover {
		text-decoration: underline;
	}

	@media (max-width: 768px) {
		.form-row {
			grid-template-columns: 1fr;
		}
	}
</style>
