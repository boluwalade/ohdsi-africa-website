<script lang="ts">
	import PageHeader from '$lib/components/PageHeader.svelte';
	import ContactForm from '$lib/components/ContactForm.svelte';
</script>

<svelte:head>
	<title>Contact - OHDSI Africa</title>
	<meta name="description" content="Get in touch with the OHDSI Africa community. Contact us for collaboration, support, or general inquiries." />
</svelte:head>

<PageHeader 
	title="📩 Contact Us" 
	subtitle="Get in touch with the OHDSI Africa community"
/>

<section class="section">
	<div class="container">
		<div class="contact-layout">
			<div class="contact-info">
				<h2>Get in Touch</h2>
				<p>Have questions about joining OHDSI Africa or interested in collaboration? We'd love to hear from you! Our community is here to support your health data science journey.</p>
				
				<div class="contact-methods">
					<div class="contact-method">
						<i class="fas fa-envelope"></i>
						<div>
							<h4>General Inquiries</h4>
							<p>For general questions, collaboration opportunities, and information about OHDSI Africa</p>
							<a href="mailto:info@ohdsi-africa.org">info@ohdsi-africa.org</a>
						</div>
					</div>
					
					<div class="contact-method">
						<i class="fas fa-users"></i>
						<div>
							<h4>Community Support</h4>
							<p>Join our Teams channels and working groups for real-time support and collaboration</p>
							<a href="https://tinyurl.com/JoinOHDSI" target="_blank" rel="noopener noreferrer">Join OHDSI Community</a>
						</div>
					</div>
					
					<div class="contact-method">
						<i class="fas fa-calendar-alt"></i>
						<div>
							<h4>Biweekly Meetings</h4>
							<p>Participate in our regular Africa Chapter meetings held every two weeks</p>
							<a href="https://tinyurl.com/AfricaChapMtg" target="_blank" rel="noopener noreferrer">Join Meetings</a>
						</div>
					</div>
					
					<div class="contact-method">
						<i class="fas fa-question-circle"></i>
						<div>
							<h4>Technical Support</h4>
							<p>Get help with OMOP CDM implementation, tools, and technical questions</p>
							<a href="mailto:support@ohdsi-africa.org">support@ohdsi-africa.org</a>
						</div>
					</div>
				</div>
			</div>
			
			<div class="contact-form-section">
				<ContactForm />
			</div>
		</div>
	</div>
</section>

<style>
	.contact-layout {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: var(--spacing-3xl);
		align-items: start;
		margin-bottom: var(--spacing-3xl);
	}

	.contact-info h2 {
		font-size: var(--font-size-2xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-lg);
	}

	.contact-methods {
		margin-top: var(--spacing-xl);
	}

	.contact-method {
		display: flex;
		align-items: flex-start;
		gap: var(--spacing-lg);
		margin-bottom: var(--spacing-xl);
		padding: var(--spacing-lg);
		background: var(--white);
		border-radius: var(--radius-lg);
		box-shadow: var(--shadow-sm);
		transition: transform 0.3s ease;
	}

	.contact-method:hover {
		transform: translateY(-2px);
		box-shadow: var(--shadow-md);
	}

	.contact-method i {
		font-size: var(--font-size-xl);
		color: var(--secondary-blue);
		margin-top: var(--spacing-xs);
		flex-shrink: 0;
	}

	.contact-method h4 {
		font-size: var(--font-size-lg);
		font-weight: 600;
		color: var(--primary-blue);
		margin-bottom: var(--spacing-xs);
	}

	.contact-method a {
		color: var(--secondary-blue);
		text-decoration: none;
		font-weight: 500;
		transition: color 0.3s ease;
	}

	.contact-method a:hover {
		color: var(--primary-blue);
	}

	.contact-form-section {
		background: var(--white);
		padding: var(--spacing-2xl);
		border-radius: var(--radius-lg);
		box-shadow: var(--shadow-md);
	}

	@media (max-width: 1024px) {
		.contact-layout {
			grid-template-columns: 1fr;
			gap: var(--spacing-2xl);
		}
	}
</style>
