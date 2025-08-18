<script lang="ts">
	import PageHeader from '$lib/components/PageHeader.svelte';
</script>

<svelte:head>
	<title>2025 Africa Symposium - OHDSI Africa</title>
	<meta name="description" content="Join us for the inaugural OHDSI Africa Symposium, November 10-12, 2025 in Kampala, Uganda." />
</svelte:head>

<PageHeader 
	title="🌍 2025 Africa Symposium" 
	subtitle="Join Us At The Inaugural OHDSI Africa Symposium"
/>

<section class="section">
	<div class="container">
		<!-- Hero Event Info -->
		<div class="card" style="background: linear-gradient(135deg, var(--light-blue), var(--white)); margin-bottom: var(--spacing-3xl); text-align: center;">
			<h2 style="color: var(--primary-blue); font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">
				November 10-12, 2025
			</h2>
			<p style="font-size: var(--font-size-lg); margin-bottom: var(--spacing-md);">
				<strong>Joint Clinical Research Centre (JCRC) & Mestil Hotel Kampala</strong>
			</p>
			<p style="margin-bottom: var(--spacing-xl);">
				The inaugural OHDSI Africa Symposium will be held in Kampala at the Joint Clinical Research Centre (JCRC) and Mestil Hotel. Our community is delighted to introduce a new face-to-face opportunity in Africa, where OHDSI is growing at an exciting pace. We hope you will join us for this historical moment.
			</p>
			<a href="https://www.ohdsi.org/africa2025/" class="btn btn-primary" target="_blank" rel="noopener noreferrer">
				<i class="fas fa-external-link-alt"></i> Register for OHDSI Africa 2025
			</a>
		</div>

		<!-- Important Dates -->
		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Important Dates</h3>
			<div class="card-grid" style="grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));">
				<div class="card card-center" style="background: var(--light-gray); border-left: 4px solid var(--warning);">
					<i class="fas fa-calendar-alt" style="font-size: var(--font-size-2xl); color: var(--warning); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title">Collaborator Showcase</h4>
					<ul style="text-align: left; margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);"><strong>Submissions deadline:</strong> September 10</li>
						<li style="margin-bottom: var(--spacing-xs);"><strong>Submissions review:</strong> September 11-30</li>
						<li style="margin-bottom: var(--spacing-xs);"><strong>Notification of acceptance:</strong> October 5</li>
					</ul>
				</div>

				<div class="card card-center" style="background: var(--light-gray); border-left: 4px solid var(--success);">
					<i class="fas fa-calendar-check" style="font-size: var(--font-size-2xl); color: var(--success); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title">Symposium Schedule</h4>
					<ul style="text-align: left; margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);"><strong>Tutorial:</strong> November 10 at JCRC</li>
						<li style="margin-bottom: var(--spacing-xs);"><strong>Main conference:</strong> November 11-12 at Mestil Hotel</li>
					</ul>
				</div>

				<div class="card card-center" style="background: var(--light-gray); border-left: 4px solid var(--primary-blue);">
					<i class="fas fa-hotel" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title">Mestil Hotel Accommodations</h4>
					<ul style="text-align: left; margin-top: var(--spacing-md);">
						<li style="margin-bottom: var(--spacing-xs);"><strong>Booking Code:</strong> JCRC</li>
						<li style="margin-bottom: var(--spacing-sm);">
							<a href="https://direct-book.com/properties/MestilDIRECT?promotion%5Fcode=JCRC25" target="_blank" rel="noopener noreferrer" class="btn btn-secondary" style="font-size: var(--font-size-sm); padding: var(--spacing-sm) var(--spacing-md);">
								<i class="fas fa-bed"></i> Book Hotel
							</a>
						</li>
					</ul>
				</div>
			</div>
		</div>

		<!-- Event Structure -->
		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Event Structure</h3>
			<p style="text-align: center; max-width: 800px; margin: 0 auto var(--spacing-xl);">
				The first OHDSI Africa symposium will be hosted by JCRC and will begin with a dedicated one-day training course at JCRC, followed by a two-day main conference at Mestil hotel.
			</p>
			<div class="card-grid-sm">
				<div class="card" style="display: flex; gap: var(--spacing-lg); align-items: center;">
					<div style="width: 50px; height: 50px; background: var(--primary-blue); color: var(--white); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; flex-shrink: 0; font-size: var(--font-size-lg);">1</div>
					<div>
						<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-sm);">Tutorial Day</h4>
						<p><strong>November 10</strong> - Dedicated one-day training course at Joint Clinical Research Centre (JCRC)</p>
					</div>
				</div>

				<div class="card" style="display: flex; gap: var(--spacing-lg); align-items: center;">
					<div style="width: 50px; height: 50px; background: var(--secondary-blue); color: var(--white); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-weight: 700; flex-shrink: 0; font-size: var(--font-size-lg);">2</div>
					<div>
						<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-sm);">Main Conference</h4>
						<p><strong>November 11-12</strong> - Two-day symposium at Mestil Hotel featuring presentations, networking, and collaboration</p>
					</div>
				</div>
			</div>
		</div>

		<!-- Collaborator Showcase -->
		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Collaborator Showcase</h3>
			<div class="section-intro">
				<p>
					The inaugural OHDSI Africa Symposium will showcase how African health data can be standardized and leveraged for high quality research and policy. It will demonstrate real-world examples of OHDSI's impact on health outcomes and build capacity for African institutions to participate in global research networks.
				</p>
			</div>

			<div class="card-grid" style="margin-top: var(--spacing-xl);">
				<div class="card">
					<i class="fas fa-presentation" style="font-size: var(--font-size-3xl); color: var(--secondary-blue); margin-bottom: var(--spacing-lg);"></i>
					<h4 class="card-title">Lightning Talks</h4>
					<p>6-minute podium presentations to verbally share your story with the community. Perfect for sharing research findings, implementation experiences, and innovative approaches.</p>
				</div>

				<div class="card">
					<i class="fas fa-image" style="font-size: var(--font-size-3xl); color: var(--secondary-blue); margin-bottom: var(--spacing-lg);"></i>
					<h4 class="card-title">Poster Sessions</h4>
					<p>Static display summaries of your latest research. Maximum size: 48 inches long X 36 inches high, horizontal orientation. Both traditional and Mike Morrison poster templates available.</p>
				</div>
			</div>
		</div>

		<!-- Strategic Focus Areas -->
		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">OHDSI Strategic Focus Areas</h3>
			<p style="text-align: center; max-width: 800px; margin: 0 auto var(--spacing-xl);">
				Topics should align with at least one of OHDSI's strategic areas of focus:
			</p>
			<div class="card-grid-sm">
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-database" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Data Standards</h4>
					<p>Observational data standards and management</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-chart-line" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Methodological Research</h4>
					<p>Research methodology and statistical approaches</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-code" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Open-Source Analytics</h4>
					<p>Development of analytical tools and platforms</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-user-md" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Clinical Applications</h4>
					<p>Real-world clinical implementations and use cases</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-users" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Community Development</h4>
					<p>Building and strengthening research communities</p>
				</div>
				
				<div class="card card-center" style="background: var(--light-gray); border-top: 4px solid var(--secondary-blue);">
					<i class="fas fa-stethoscope" style="font-size: var(--font-size-2xl); color: var(--primary-blue); margin-bottom: var(--spacing-md);"></i>
					<h4 class="card-title" style="margin-bottom: var(--spacing-sm);">Clinical Research</h4>
					<p>Characterization, estimation, and prediction studies</p>
				</div>
			</div>
		</div>

		<!-- Submission Guidelines -->
		<div style="margin-bottom: var(--spacing-3xl);">
			<h3 class="section-title" style="font-size: var(--font-size-2xl); margin-bottom: var(--spacing-lg);">Submission Guidelines</h3>
			<div class="card" style="background: var(--light-blue); padding: var(--spacing-2xl);">
				<h4 style="color: var(--primary-blue); margin-bottom: var(--spacing-lg);">Document Requirements</h4>
				<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: var(--spacing-lg);">
					<ul style="list-style: disc; padding-left: var(--spacing-lg);">
						<li style="margin-bottom: var(--spacing-sm);">Maximum 1,000 words (exclusive of references/authors/affiliations/tables/figures)</li>
						<li style="margin-bottom: var(--spacing-sm);">Maximum 10 MB file size</li>
						<li style="margin-bottom: var(--spacing-sm);">PDF format required</li>
						<li style="margin-bottom: var(--spacing-sm);">Clearly state all co-authors and affiliations</li>
					</ul>
					<ul style="list-style: disc; padding-left: var(--spacing-lg);">
						<li style="margin-bottom: var(--spacing-sm);">Required sections: Background, Methods, Results, Conclusion</li>
						<li style="margin-bottom: var(--spacing-sm);">Maximum 15 references (Vancouver, DOI, or PMID style)</li>
						<li style="margin-bottom: var(--spacing-sm);">Unpublished work only (preprints acceptable)</li>
						<li style="margin-bottom: var(--spacing-sm);">Up to 3 additional figures/tables allowed</li>
					</ul>
				</div>
				<div style="margin-top: var(--spacing-xl); text-align: center;">
					<p style="margin-bottom: var(--spacing-lg);">
						<strong>Naming Convention:</strong> lastname-firstname_title_2025symposium.pdf
					</p>
					<a href="https://www.ohdsi.org/africa2025/" class="btn btn-primary" target="_blank" rel="noopener noreferrer">
						<i class="fas fa-upload"></i> Submit Your Work
					</a>
				</div>
			</div>
		</div>

		<!-- Contact Information -->
		<div style="text-align: center; background: linear-gradient(135deg, var(--light-blue), var(--white)); padding: var(--spacing-2xl); border-radius: var(--radius-lg);">
			<h3 style="color: var(--primary-blue); margin-bottom: var(--spacing-md); font-size: var(--font-size-2xl);">Join This Historic Moment</h3>
			<p style="margin-bottom: var(--spacing-lg); max-width: 600px; margin-left: auto; margin-right: auto;">
				Be part of the inaugural OHDSI Africa Symposium and help shape the future of health data research across the African continent.
			</p>
			<div style="display: flex; gap: var(--spacing-lg); justify-content: center; flex-wrap: wrap;">
				<a href="https://www.ohdsi.org/africa2025/" class="btn btn-primary" target="_blank" rel="noopener noreferrer">
					<i class="fas fa-ticket-alt"></i> Register Now
				</a>
				<a href="/contact" class="btn btn-secondary">
					<i class="fas fa-envelope"></i> Contact Us
				</a>
			</div>
			<p style="margin-top: var(--spacing-lg); font-size: var(--font-size-sm); color: var(--medium-gray);">
				For questions about submissions or the symposium, please contact the organizing committee.
			</p>
		</div>
	</div>
</section>
