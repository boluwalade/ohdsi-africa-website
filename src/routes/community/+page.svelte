<script lang="ts">
	import PageHeader from '$lib/components/PageHeader.svelte';
</script>

<svelte:head>
	<title>Community - OHDSI Africa</title>
	<meta name="description" content="Connect with researchers, clinicians, and data scientists across Africa through the OHDSI Africa community." />
</svelte:head>

<PageHeader 
	title="👥 Join Our Community" 
	subtitle="Connect with researchers, clinicians, and data scientists across Africa"
/>

<section class="section">
	<div class="container">
		<div class="section-intro">
			<h2>Welcome to the OHDSI Africa Community</h2>
			<p>Join our growing network of healthcare professionals, researchers, and data scientists working together to improve healthcare outcomes across Africa through collaborative research and standardized data models.</p>
		</div>

		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">How to Get Involved</h3>
			<div class="card-grid" style="grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));">
				<div class="card card-center">
					<i class="card-icon fas fa-users"></i>
					<h4 class="card-title">Join OHDSI Global</h4>
					<p>Become part of the global OHDSI community and access resources, tools, and collaboration opportunities worldwide.</p>
					<a href="https://tinyurl.com/JoinOHDSI" class="btn btn-primary" target="_blank" rel="noopener noreferrer">
						<i class="fas fa-external-link-alt"></i> Join Now
					</a>
				</div>
				
				<div class="card card-center">
					<i class="card-icon fas fa-calendar-alt"></i>
					<h4 class="card-title">Africa Chapter Meetings</h4>
					<p>Participate in our biweekly meetings to stay updated on developments and collaborate with African researchers.</p>
					<a href="https://tinyurl.com/AfricaChapMtg" class="btn btn-primary" target="_blank" rel="noopener noreferrer">
						<i class="fas fa-video"></i> Join Meetings
					</a>
				</div>
			</div>
		</div>

		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Community Platforms & Resources</h3>
			<div class="card-grid">
				<div class="card">
					<i class="card-icon fab fa-microsoft"></i>
					<h4 class="card-title">MS Teams Groups</h4>
					<p>Join our Microsoft Teams channels for real-time discussions, project updates, and collaborative work with African researchers.</p>
					<ul style="list-style: disc; padding-left: var(--spacing-lg); margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);">General discussion channels</li>
						<li style="margin-bottom: var(--spacing-xs);">Regional working groups</li>
						<li style="margin-bottom: var(--spacing-xs);">Project-specific teams</li>
						<li style="margin-bottom: var(--spacing-xs);">Technical support channels</li>
					</ul>
				</div>
				
				<div class="card">
					<i class="card-icon fas fa-project-diagram"></i>
					<h4 class="card-title">Working Groups</h4>
					<p>Participate in specialized working groups focused on different aspects of health data research and implementation.</p>
					<ul style="list-style: disc; padding-left: var(--spacing-lg); margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);">Data Standards Working Group</li>
						<li style="margin-bottom: var(--spacing-xs);">Training & Education Committee</li>
						<li style="margin-bottom: var(--spacing-xs);">Implementation Support Team</li>
						<li style="margin-bottom: var(--spacing-xs);">Research Collaboration Network</li>
					</ul>
				</div>
				
				<div class="card">
					<i class="card-icon fas fa-star"></i>
					<h4 class="card-title">Project Spotlights</h4>
					<p>Showcase your research, learn from success stories, and get recognition for innovative work across African institutions.</p>
					<ul style="list-style: disc; padding-left: var(--spacing-lg); margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);">Monthly project presentations</li>
						<li style="margin-bottom: var(--spacing-xs);">Research publication highlights</li>
						<li style="margin-bottom: var(--spacing-xs);">Implementation success stories</li>
						<li style="margin-bottom: var(--spacing-xs);">Innovation showcases</li>
					</ul>
				</div>
				
				<div class="card">
					<i class="card-icon fas fa-building"></i>
					<h4 class="card-title">Member Directory</h4>
					<p>Connect with researchers and organizations through our Teams member directory and networking platform.</p>
					<ul style="list-style: disc; padding-left: var(--spacing-lg); margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);">Searchable member profiles</li>
						<li style="margin-bottom: var(--spacing-xs);">Institution listings</li>
						<li style="margin-bottom: var(--spacing-xs);">Expertise matching</li>
						<li style="margin-bottom: var(--spacing-xs);">Collaboration opportunities</li>
					</ul>
				</div>
			</div>
		</div>

		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Member Benefits</h3>
			<div class="card-grid-sm">
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-graduation-cap" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Training & Education</h4>
					<p>Access to OHDSI training materials, EHDEN Academy courses, and specialized African health data workshops.</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-handshake" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Collaboration Opportunities</h4>
					<p>Connect with researchers across Africa and globally for multi-institutional studies and data sharing initiatives.</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-tools" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Access to Tools</h4>
					<p>Free access to all OHDSI tools including ATLAS, ATHENA, Achilles, and the full HADES R package ecosystem.</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-support" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Technical Support</h4>
					<p>Get help with OMOP CDM implementation, data transformation, and analysis from experienced community members.</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-conference" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Events & Conferences</h4>
					<p>Priority access to OHDSI symposiums, workshops, and special events both in Africa and globally.</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-certificate" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Recognition</h4>
					<p>Opportunities for research publication, presentation at conferences, and recognition within the global OHDSI community.</p>
				</div>
			</div>
		</div>

		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Getting Started</h3>
			<div class="card-grid-sm">
				<div class="card" style="display: flex; gap: var(--spacing-lg);">
					<div style="width: 40px; height: 40px; background: var(--primary-blue); color: var(--white); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; flex-shrink: 0;">1</div>
					<div>
						<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-sm);">Join the Global Community</h4>
						<p>Sign up for OHDSI global membership to access tools and resources.</p>
					</div>
				</div>
				
				<div class="card" style="display: flex; gap: var(--spacing-lg);">
					<div style="width: 40px; height: 40px; background: var(--primary-blue); color: var(--white); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; flex-shrink: 0;">2</div>
					<div>
						<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-sm);">Attend Africa Chapter Meetings</h4>
						<p>Join our biweekly meetings to meet the community and learn about ongoing projects.</p>
					</div>
				</div>
				
				<div class="card" style="display: flex; gap: var(--spacing-lg);">
					<div style="width: 40px; height: 40px; background: var(--primary-blue); color: var(--white); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; flex-shrink: 0;">3</div>
					<div>
						<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-sm);">Join Working Groups</h4>
						<p>Participate in specialized groups that match your interests and expertise.</p>
					</div>
				</div>
				
				<div class="card" style="display: flex; gap: var(--spacing-lg);">
					<div style="width: 40px; height: 40px; background: var(--primary-blue); color: var(--white); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; flex-shrink: 0;">4</div>
					<div>
						<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-sm);">Start Collaborating</h4>
						<p>Begin working on projects, sharing resources, and contributing to the community.</p>
					</div>
				</div>
			</div>
		</div>

		<div style="text-align: center; background: linear-gradient(135deg, var(--light-blue), var(--white)); padding: var(--spacing-2xl); border-radius: var(--radius-lg);">
			<h3 style="color: var(--primary-blue); margin-bottom: var(--spacing-md); font-size: var(--font-size-2xl);">Ready to Join?</h3>
			<p style="margin-bottom: var(--spacing-lg);">Take the first step towards transforming healthcare research in Africa through collaborative data science.</p>
			<div style="display: flex; gap: var(--spacing-lg); justify-content: center; flex-wrap: wrap;">
				<a href="https://tinyurl.com/JoinOHDSI" class="btn btn-primary" target="_blank" rel="noopener noreferrer">Join OHDSI Community</a>
				<a href="/contact" class="btn btn-secondary">Contact Us</a>
			</div>
		</div>
	</div>
</section>


