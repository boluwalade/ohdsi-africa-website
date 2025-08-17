<script lang="ts">
	import PageHeader from '$lib/components/PageHeader.svelte';
</script>

<svelte:head>
	<title>About - OHDSI Africa</title>
	<meta name="description" content="Learn about OHDSI Africa's mission, leadership, and vision for collaborative health data research across the African continent." />
</svelte:head>

<PageHeader 
	title="About OHDSI Africa" 
	subtitle="Building collaborative health data research across the African continent"
/>

<section class="section">
	<div class="container">
		<div class="about-grid">
			<div class="about-card">
				<h3>Our Mission</h3>
				<p>To build a strong African OHDSI community that actively contributes to the extension of the OMOP-CDM and analytical tool development, promoting collaborative research across the continent.</p>
			</div>
			<div class="about-card">
				<h3>Global Network</h3>
				<p>OHDSI Africa is part of the global OHDSI network with coordinating centers worldwide, bringing out the value of health data through large-scale analytics.</p>
			</div>
			<div class="about-card">
				<h3>Open Source Solutions</h3>
				<p>All our solutions are open-source, ensuring accessibility and transparency in health data research across African institutions.</p>
			</div>
		</div>
		
		<div class="leadership-section">
			<h3>Leadership & Governance</h3>
			<p>Our steering committee includes representatives from leading African institutions and healthcare organizations, working together to advance observational health data sciences across the continent.</p>
			
			<div class="leadership-grid">
				<div class="leader-card">
					<h4>Steering Committee</h4>
					<p>Representatives from major African research institutions, universities, and healthcare organizations guide the strategic direction of OHDSI Africa.</p>
				</div>
				<div class="leader-card">
					<h4>Working Groups</h4>
					<p>Specialized teams focusing on data standards, tool development, training, and implementation across different African regions.</p>
				</div>
				<div class="leader-card">
					<h4>Institutional Partners</h4>
					<p>A growing network of hospitals, research centers, and health ministries collaborating on data standardization and research.</p>
				</div>
			</div>
		</div>

		<div class="global-context">
			<h3>OHDSI Global Context</h3>
			<p>The Observational Health Data Sciences and Informatics (OHDSI, pronounced "Odyssey") program is a multi-stakeholder, interdisciplinary collaborative to bring out the value of health data through large-scale analytics. OHDSI aims to generate accurate, reproducible, and well-calibrated evidence and promote better health decisions and better care.</p>
			
			<div class="global-stats">
				<div class="stat-card">
					<h4>Global Reach</h4>
					<p>OHDSI has established a global network of researchers and observational health databases with coordinating centers on every continent.</p>
				</div>
				<div class="stat-card">
					<h4>Central Coordination</h4>
					<p>The main coordinating center is housed at Columbia University, with regional chapters providing local leadership and support.</p>
				</div>
			</div>
		</div>

		<div class="publications-section">
			<h3>Key Resources & Publications</h3>
			<div class="resources-grid">
				<div class="resource-item">
					<i class="fas fa-book"></i>
					<h4>OHDSI Publications</h4>
					<p>Contributing to the global body of OHDSI research with African perspectives and use cases.</p>
				</div>
				<div class="resource-item">
					<i class="fas fa-database"></i>
					<h4>OMOP CDM</h4>
					<p>A Digital Square Content Global Good for standardized health data, enabling interoperability across African health systems.</p>
				</div>
				<div class="resource-item">
					<i class="fas fa-users"></i>
					<h4>Collaborative Research</h4>
					<p>Cross-institutional studies and data sharing initiatives advancing African health research capabilities.</p>
				</div>
				<div class="resource-item">
					<i class="fas fa-map-marked-alt"></i>
					<h4>African Context</h4>
					<p>Developing region-specific implementations that address unique African healthcare challenges and opportunities.</p>
				</div>
			</div>
		</div>
	</div>
</section>

<style>
	.about-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
		gap: var(--spacing-xl);
		margin-bottom: var(--spacing-2xl);
	}

	.about-card {
		background: var(--white);
		padding: var(--spacing-xl);
		border-radius: var(--radius-lg);
		box-shadow: var(--shadow-md);
		transition: transform 0.3s ease, box-shadow 0.3s ease;
	}

	.about-card:hover {
		transform: translateY(-4px);
		box-shadow: var(--shadow-lg);
	}

	.about-card h3 {
		font-size: var(--font-size-xl);
		font-weight: 600;
		color: var(--primary-blue);
		margin-bottom: var(--spacing-md);
	}

	.leadership-section,
	.publications-section,
	.global-context {
		background: var(--white);
		padding: var(--spacing-xl);
		border-radius: var(--radius-lg);
		box-shadow: var(--shadow-md);
		margin-bottom: var(--spacing-xl);
	}

	.leadership-section h3,
	.publications-section h3,
	.global-context h3 {
		font-size: var(--font-size-xl);
		font-weight: 600;
		color: var(--primary-blue);
		margin-bottom: var(--spacing-md);
	}

	.leadership-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: var(--spacing-lg);
		margin-top: var(--spacing-lg);
	}

	.leader-card {
		background: var(--light-gray);
		padding: var(--spacing-lg);
		border-radius: var(--radius-md);
		border-left: 4px solid var(--primary-blue);
	}

	.leader-card h4 {
		color: var(--primary-blue);
		margin-bottom: var(--spacing-sm);
	}

	.global-stats {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
		gap: var(--spacing-lg);
		margin-top: var(--spacing-lg);
	}

	.stat-card {
		background: var(--white);
		padding: var(--spacing-lg);
		border-radius: var(--radius-lg);
		box-shadow: var(--shadow-sm);
		border-top: 4px solid var(--secondary-blue);
	}

	.stat-card h4 {
		color: var(--primary-blue);
		margin-bottom: var(--spacing-sm);
	}

	.resources-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: var(--spacing-lg);
		margin-top: var(--spacing-lg);
	}

	.resource-item {
		text-align: center;
		padding: var(--spacing-lg);
	}

	.resource-item i {
		font-size: var(--font-size-3xl);
		color: var(--secondary-blue);
		margin-bottom: var(--spacing-md);
	}

	.resource-item h4 {
		color: var(--primary-blue);
		margin-bottom: var(--spacing-sm);
	}
</style>
