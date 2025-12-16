<script lang="ts">
	import { browser } from '$app/environment';
	
	let expandedSection: string | null = null;
	let expandedSpeaker: string | null = null;
	let expandedVenue: string | null = null;
	let currentSlide = 0;
	const speakersPerSlide = 6;
	
	// Speaker data with bios
	const speakers = [
		{ id: 'ryan', initials: 'PR', name: 'Dr. Patrick Ryan', title: 'VP of Observational Health Data Analytics, Janssen R&D; OHDSI Founding Collaborator', image: '/speakers/patrick-ryan.jpg', bio: 'Dr. Patrick Ryan is the Vice President of Observational Health Data Analytics at Janssen Research and Development, a division of Johnson & Johnson. He leads efforts to develop and apply advanced analytical methods to better understand the real-world effects of medical products. As a founding collaborator of OHDSI, Dr. Ryan has been instrumental in establishing one of the world\'s largest networks for observational health data analysis.' },
		{ id: 'vanzandt', initials: 'MV', name: 'Mui Van Zandt', title: 'VP & Global Head of Data Strategy, IQVIA; Asia-Pacific OHDSI Chapter Lead', image: '/speakers/mui-van-zandt.jpg', bio: 'Mui Van Zandt is Vice-President & Global Head of Data Strategy at IQVIA and leads the Asia-Pacific chapter of OHDSI. With over 20 years of experience in large-scale patient data networks and real-world evidence innovation, Mui has helped drive global adoption of the OMOP common data model.' },
		{ id: 'aceng', initials: 'JA', name: 'Dr. Jane Ruth Aceng', title: 'Minister of Health, Uganda', image: '/speakers/jane-aceng.jpg', bio: 'Dr. Jane Ruth Aceng is Uganda\'s Minister of Health and a highly respected medical doctor, researcher, and public health leader. She has dedicated her career to strengthening Uganda\'s health system and improving access to quality healthcare for all.' },
		{ id: 'sung', initials: 'CS', name: 'Prof. Cynthia Sung, PhD', title: 'Adjunct Associate Professor, Duke-NUS; Co-Lead, OHDSI Africa Chapter', image: '/speakers/cynthia-sung.jpg', bio: 'Prof. Cynthia Sung is a clinical pharmacologist and biomedical engineer with a distinguished career spanning government, industry, and academia. As Co-Lead of the OHDSI Africa Chapter since 2022, Cynthia is committed to expanding FAIR data in under-represented populations. In 2023, she received the OHDSI Titan Award for Community Collaboration.' },
		{ id: 'kiragga', initials: 'AK', name: 'Dr. Agnes Kiragga', title: 'Data Science Program Lead, African Population Health Research Council', image: '/speakers/agnes-kiragga.jpg', bio: 'Agnes Kiragga leads the Data Science Program at the African Population Health Research Council in Nairobi, Kenya. With over 20 years of experience, she specializes in utilizing diverse data, including African longitudinal population cohorts. She leads the Data Science Without Borders (DSWB) multi-country project.' },
		{ id: 'asiimwe', initials: 'AA', name: 'Alex Asiimwe', title: 'Head of Evidence Generation, Innovation, and Partnerships, Gilead Sciences', image: '/speakers/alex-asiimwe.jpg', bio: 'Alex is a biopharmaceutical executive and health research leader with extensive experience in evidence generation, innovation, and strategic partnerships across the globe. At Gilead Sciences, he drives initiatives that leverage real-world data and develop innovative health solutions.' },
		{ id: 'kanter', initials: 'AK', name: 'Dr. Andrew S. Kanter', title: 'Assistant Professor, Columbia University; Strategic Advisor, IMO Health', image: '/speakers/andrew-kanter.jpg', bio: 'Dr. Andrew S. Kanter is a distinguished leader in health informatics and global health. He directs the Columbia International eHealth Lab (CIEL) and is the Terminology and Metadata lead for OpenMRS, working closely with OHDSI, Open Concept Lab and OpenHIE communities.' },
		{ id: 'muyingo', initials: 'SM', name: 'Dr. Sylvia Muyingo', title: 'Research Scientist, African Population and Health Research Centre (APHRC)', image: '/speakers/silvia-muyingo.jpg', bio: 'Dr. Sylvia Muyingo is a research scientist at APHRC in Nairobi. Her expertise spans data harmonisation, longitudinal health data systems, and translating complex datasets into actionable public-health insights. Recently, she has focused on mental health data across Africa.' },
		{ id: 'burn', initials: 'EB', name: 'Dr. Edward Burn', title: 'Senior Researcher, University of Oxford; DARWIN-EU & HERON-UK Projects', image: '/speakers/edward-burn.jpg', bio: 'Dr. Edward Burn is a senior researcher at the University of Oxford, primarily working on the DARWIN-EU and HERON-UK projects. He is a long-standing contributor to OHDSI network studies and open-source software.' },
		{ id: 'ario', initials: 'AA', name: 'Prof. Dr. Alex Riolexus Ario', title: 'Program Director, Uganda Public Health Fellowship Program; Director, Uganda National Institute of Public Health', image: '/speakers/alex-ario.jpg', bio: 'Prof. Dr. Alex Riolexus Ario is a Medical Doctor and Public Health Specialist. He serves as Editor in Chief of the Uganda Public Health Bulletin and has published about 300 papers in peer-reviewed journals. He is currently Program Director of the Uganda Public Health Fellowship Program.' },
		{ id: 'herbst', initials: 'KH', name: 'Dr. Kobus Herbst', title: 'Director of Population Science, Africa Health Research Institute (AHRI)', image: '/speakers/kobus-herbst.jpg', bio: 'Dr. Kobus Herbst is Director of Population Science at AHRI. He has led the development of large-scale longitudinal health and demographic surveillance systems in South Africa and is a co-leader of the African Population Cohorts Consortium (APCC).' },
		{ id: 'nakazibwe', initials: 'BN', name: 'Brenda Nakazibwe', title: 'Team Lead, Pathogen Economy, STIS, Uganda', image: 'speakers/brenda-nakazibwe.webp', bio: 'Brenda Nakazibwe is the Team Lead for the Pathogen Economy at the Science, Technology & Innovation Secretariat under the Office of the President of Uganda. Her work focuses on building local capacity around diagnostics, therapeutics and vaccine development.' },
		{ id: 'ganda', initials: 'GG', name: 'Dr. Gregory Ganda', title: 'County Executive Committee Member for Health, Kisumu County, Kenya', image: '/speakers/gregory-ganda.jpg', bio: 'Dr. Gregory Ganda serves as the CECM for Health in Kisumu County, Kenya. His leadership has been pivotal in reducing maternal mortality by 30% over five years through data-driven health interventions.' },
		{ id: 'waynee', initials: 'SW', name: 'Steven Waynee', title: 'Founder & CEO, IntelliSOFT; President, HELINA', image: '/speakers/steven-waynee.jpg', bio: 'Steven Waynee is a Kenyan digital health expert with over 15 years of experience. He is Founder & CEO of IntelliSOFT Consulting Ltd and President of the Pan African Health Informatics Association (HELINA).' },
		{ id: 'mbaka', initials: 'PM', name: 'Paul Mbaka', title: 'Assistant Commissioner, Health Information, Ministry of Health, Uganda', image: '/speakers/paul-mbaka.jpg', bio: 'Paul Mbaka serves as Assistant Commissioner in the Department of Health Information at the Ministry of Health, Uganda. He has been instrumental in the design and deployment of digital health platforms within Uganda\'s health system.' },
		{ id: 'bouras', initials: 'AB', name: 'Dr. Adam Bouras', title: 'Founder, Tritonis Inc.; Public Health Informatics Fellow, CDC', image: '/speakers/adam-bouras.jpg', bio: 'Dr. Adam Bouras is the founder of Tritonis Inc. and a researcher in health informatics and public health. He is actively involved with the OHDSI community, helping bridge local medication coding systems and global standards.' },
		{ id: 'walravens', initials: 'MW', name: 'Michel Walravens', title: 'Rheumatologist & Researcher, Hasselt University', image: '/speakers/michel-walravens.jpg', bio: 'Michel Walravens is developing OikoLexis—a web-based platform that supports the community-driven translation of the OHDSI Book into any language, including French, Arabic, Portuguese, and Kiswahili.' },
		{ id: 'atwine', initials: 'MA', name: 'Mugume Atwine', title: 'Data Scientist, Uganda', image: '/speakers/mugume-atwine.jpg', bio: 'Mugume Twinamatsiko Atwine is a Ugandan data scientist with over five years of experience in big-data analytics and machine learning applied to public-health challenges.' },
		{ id: 'tamirat', initials: 'BT', name: 'Bekure Tamirat', title: 'Data Science & Analytics Unit Lead, Africa CDC', image: '/speakers/bekure-tamirat.jpg', bio: 'Bekure Tamirat serves as the Data Science & Analytics Unit Lead at Africa CDC, advancing data-driven public health strategies across the continent.' },
		{ id: 'bagarukayo', initials: 'KB', name: 'Kenneth Bagarukayo', title: 'Commissioner ICT R&D, Ministry of ICT, Uganda', image: '/speakers/kenneth-bagarukayo.jpg', bio: 'Kenneth Bagarukayo is a highly experienced ICT leader serving as Commissioner ICT Research and Development at the Ministry of ICT and National Guidance.' },
		{ id: 'kadengye', initials: 'DK', name: 'Dr. Damazo T. Kadengye', title: 'Head of Data, Measurement & Evaluation, APHRC', image: '/speakers/damazo-kadengye.webp', bio: 'Dr. Damazo Kadengye is a multidisciplinary researcher and statistician specializing in public health, education, and epidemiology at APHRC.' },
		{ id: 'tamale', initials: 'WT', name: 'Dr. William Tamale', title: 'Clinical Manager, JCRC', image: '/speakers/william-tamale.png', bio: 'Dr. William Tamale is a medical doctor and research leader with over 20 years of experience in clinical care and health research at JCRC.' },
		{ id: 'taylor', initials: 'AT', name: 'Amelia Taylor', title: 'Research Fellow, University of Nottingham', image: '/speakers/amelia-taylor.png', bio: 'Amelia Taylor is a Research Fellow in primary health care at the University of Nottingham, focusing on data science and natural language processing for medicines safety.' },
		{ id: 'martufi', initials: 'VM', name: 'Valentina Martufi', title: 'Researcher, Fiocruz, Brazil', image: '/speakers/valentina-martufi.jpg', bio: 'Valentina Martufi is a researcher at CIDACS of Fiocruz Bahia, Brazil, focusing on using real-world data to improve public health research in low- and middle-income countries.' },
		{ id: 'weerasinghe', initials: 'JW', name: 'Jayasanka Weerasinghe', title: 'Product Quality Engineering Lead, OpenMRS', image: '/speakers/jayasanka-weerasinghe.jpg', bio: 'Jayasanka Weerasinghe is a Software Engineer and Product Quality Engineering Lead at OpenMRS, strengthening digital health systems through open-source collaboration.' },
		{ id: 'fankoua', initials: 'LF', name: 'Luc Baudoin Fankoua', title: 'Health Informatics Professional, Douala General Hospital, Cameroon', image: '/speakers/luc-baudoin-fankoua.jpg', bio: 'Luc Baudoin Fankoua leads digital transformation and OHDSI implementation at Douala General Hospital, demonstrating how local health institutions can join global data networks.' },
		{ id: 'saura', initials: 'AS', name: 'Dr. Anna Saura Lázaro', title: 'Senior Researcher, University of Oxford', image: '/speakers/anna-saura-lazaro.webp', bio: 'Dr. Anna Saura Lázaro is a preventive medicine specialist currently working as a Senior Researcher in real-world evidence at the University of Oxford.' },
		{ id: 'belmans', initials: 'LB', name: 'Dr. Luc Belmans', title: 'CEO, Medaman', image: '/speakers/luc-belmans.jpg', bio: 'Dr. Luc Belmans is CEO of Medaman, supporting hospitals in converting data into the OMOP CDM for better analysis, federated research, and evidence-based decision-making.' },
		{ id: 'kanyike', initials: 'FK', name: 'Dr. Francis Kanyike', title: 'Public Health Specialist, JCRC, Uganda', image: '/speakers/francis-kanyike.jpg', bio: 'Dr. Francis Kanyike is a public health specialist at JCRC with extensive experience in HIV/AIDS care, prevention, and clinical research.' },
		{ id: 'descamps', initials: 'FD', name: 'Freija Descamps', title: 'Managing Partner, edenceHealth NV', image: '/speakers/freija-descamps.jpg', bio: 'Freija Descamps leads edenceHealth NV, specialising in transforming healthcare data into actionable insights through harmonisation and federated networks.' }
	];
	
	$: totalSlides = Math.ceil(speakers.length / speakersPerSlide);
	$: slideSpeakers = Array.from({ length: totalSlides }, (_, i) => 
		speakers.slice(i * speakersPerSlide, (i + 1) * speakersPerSlide)
	);
	$: currentSpeaker = speakers.find(s => s.id === expandedSpeaker);
	
	function toggleSection(section: string) {
		expandedSection = expandedSection === section ? null : section;
	}

	function toggleSpeaker(speakerId: string) {
		expandedSpeaker = expandedSpeaker === speakerId ? null : speakerId;
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape' && expandedSpeaker) {
			expandedSpeaker = null;
		}
	}

	function toggleVenue(venue: string) {
		expandedVenue = expandedVenue === venue ? null : venue;
	}
	
	function nextSlide() {
		if (currentSlide < totalSlides - 1) {
			currentSlide++;
			scrollToSlide(currentSlide);
		}
	}
	
	function prevSlide() {
		if (currentSlide > 0) {
			currentSlide--;
			scrollToSlide(currentSlide);
		}
	}
	
	function goToSlide(index: number) {
		currentSlide = index;
		scrollToSlide(index);
	}

	function scrollToSlide(index: number) {
		if (!browser) return;
		const slider = document.querySelector('.speakers-slider');
		const grids = slider?.querySelectorAll('.speakers-grid');
		if (grids && grids[index]) {
			grids[index].scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'start' });
		}
	}

	$: if (browser) {
		document.body.style.overflow = expandedSpeaker ? 'hidden' : '';
	}
</script>

<svelte:window on:keydown={handleKeydown} />

<svelte:head>
	<title>2025 Africa Symposium - OHDSI Africa</title>
	<meta name="description" content="Join us for the inaugural OHDSI Africa Symposium, November 10-12, 2025 in Kampala, Uganda." />
</svelte:head>

<style>
	/* Shared Section Container */
	.about-section,
	.video-section,
	.programme-intro,
	.venue-section,
	.speakers-intro {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 var(--spacing-xl);
	}

	.about-section,
	.video-section,
	.programme-intro,
	.speakers-intro {
		text-align: center;
	}

	/* Shared Section Title */
	.section-title,
	.about-section h2,
	.programme-intro h2,
	.speakers-intro h2,
	.venue-accordion-wrapper h2 {
		font-size: var(--font-size-2xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-lg);
		font-weight: 700;
		border-bottom: 2px solid var(--disabled);
		padding-bottom: var(--spacing-sm);
	}

	.section-title {
		text-align: center;
		margin-bottom: var(--spacing-2xl);
	}

	/* Shared Section Intro Text */
	.about-section p,
	.programme-intro p,
	.speakers-intro p,
	.video-intro {
		font-size: var(--font-size-base);
		line-height: 1.7;
		color: var(--text-dark);
		margin-bottom: var(--spacing-lg);
	}

	.video-intro {
		color: var(--medium-gray);
		max-width: 800px;
		margin: 0 auto var(--spacing-xl);
	}

	/* Hero Banner */
	.hero-banner {
		background: linear-gradient(135deg, var(--primary-blue) 0%, var(--dark-blue) 100%);
		color: var(--white);
		padding: var(--spacing-3xl) 0;
		text-align: center;
	}

	.hero-banner .container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 var(--spacing-xl);
	}

	.hero-banner h1 {
		font-size: var(--font-size-3xl);
		margin-bottom: var(--spacing-lg);
		color: var(--white);
	}

	.hero-date-location {
		font-size: var(--font-size-lg);
		margin-bottom: var(--spacing-2xl);
		opacity: 0.95;
	}

	.hero-actions {
		display: flex;
		gap: var(--spacing-lg);
		justify-content: center;
		flex-wrap: wrap;
	}

	.btn-hero {
		background-color: var(--primary-orange);
		color: var(--white);
		padding: var(--spacing-md) var(--spacing-xl);
		border-radius: var(--radius-md);
		text-decoration: none;
		font-weight: 600;
		transition: background-color 0.2s ease;
	}

	.btn-hero:hover {
		background-color: var(--dark-orange);
	}

	/* Video Section */
	.video-wrapper {
		position: relative;
		width: 100%;
		max-width: 900px;
		margin: 0 auto;
		padding-bottom: 56.25%;
		height: 0;
		overflow: hidden;
		border-radius: var(--radius-lg);
		background: var(--dark-blue);
	}

	.video-wrapper iframe {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		border: none;
	}

	/* Carousel Navigation Buttons */
	.speakers-pager-btn {
		background: var(--primary-blue);
		color: var(--white);
		border: none;
		width: 44px;
		height: 44px;
		border-radius: 50%;
		font-size: var(--font-size-xl);
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
		transition: background-color 0.2s ease;
	}

	.speakers-pager-btn:hover:not(:disabled) {
		background: var(--primary-orange);
	}

	.speakers-pager-btn:disabled {
		opacity: 0.3;
		cursor: not-allowed;
	}

	/* Carousel Indicators */
	.speakers-indicators {
		display: flex;
		justify-content: center;
		gap: var(--spacing-sm);
		margin-top: var(--spacing-xl);
	}

	.speaker-indicator {
		width: 10px;
		height: 10px;
		border-radius: 50%;
		border: 2px solid var(--primary-blue);
		background: transparent;
		cursor: pointer;
		transition: all 0.2s ease;
	}

	.speaker-indicator.active {
		background: var(--primary-orange);
		border-color: var(--primary-orange);
	}

	/* Modal Close Button */
	.close-modal-btn {
		background: var(--white);
		border: none;
		width: 44px;
		height: 44px;
		border-radius: 50%;
		font-size: var(--font-size-xl);
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
		color: var(--primary-blue);
		transition: background-color 0.2s ease;
	}

	.close-modal-btn:hover {
		background: var(--primary-orange);
		color: var(--white);
	}

	/* Expect Grid */
	.expect-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
		gap: var(--spacing-lg);
		margin-top: var(--spacing-xl);
	}

	.expect-item {
		text-align: center;
		padding: var(--spacing-lg);
		background: var(--white);
		border-radius: var(--radius-md);
		border: 1px solid var(--light-gray);
	}

	.expect-number {
		font-size: var(--font-size-3xl);
		font-weight: 700;
		color: var(--primary-orange);
		margin-bottom: var(--spacing-xs);
	}

	.expect-label {
		font-size: var(--font-size-sm);
		color: var(--text-dark);
	}

	/* Session Cards */
	.session-card {
		background: var(--white);
		border-radius: var(--radius-md);
		margin-bottom: var(--spacing-md);
		border: 1px solid var(--light-gray);
		overflow: hidden;
	}

	.session-header {
		padding: var(--spacing-lg);
		cursor: pointer;
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		background: var(--gray-50);
		transition: background 0.2s ease;
	}

	.session-header:hover {
		background: var(--light-blue);
	}

	.session-title-group h3 {
		font-size: var(--font-size-lg);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-xs);
	}

	.session-meta {
		font-size: var(--font-size-sm);
		color: var(--medium-gray);
		margin-bottom: var(--spacing-xs);
	}

	.session-type {
		display: inline-block;
		background: var(--primary-orange);
		color: var(--white);
		padding: var(--spacing-xs) var(--spacing-sm);
		border-radius: var(--radius-sm);
		font-size: var(--font-size-xs);
		font-weight: 600;
	}

	.expand-icon {
		font-size: var(--font-size-xl);
		color: var(--primary-blue);
		transition: transform 0.2s ease;
		min-width: 44px;
		min-height: 44px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.expand-icon.expanded {
		transform: rotate(45deg);
		color: var(--primary-orange);
	}

	.session-content {
		padding: var(--spacing-lg);
		border-top: 1px solid var(--light-gray);
	}

	.session-content p,
	.session-content li {
		line-height: 1.7;
		color: var(--text-dark);
	}

	.session-content ul {
		margin-left: var(--spacing-lg);
		margin-bottom: var(--spacing-md);
	}

	.session-content li {
		margin-bottom: var(--spacing-xs);
	}

	/* Training Module */
	.training-module {
		background: var(--gray-50);
		border-left: 3px solid var(--primary-orange);
		padding: var(--spacing-lg);
		margin-bottom: var(--spacing-lg);
		border-radius: var(--radius-sm);
	}

	.training-module h5 {
		color: var(--primary-blue);
		font-size: var(--font-size-base);
		margin-bottom: var(--spacing-sm);
	}

	.training-module ul {
		margin-left: var(--spacing-lg);
		margin-bottom: var(--spacing-sm);
	}

	.training-module li {
		margin-bottom: var(--spacing-xs);
		line-height: 1.6;
	}

	.training-module em {
		color: var(--medium-gray);
		font-size: var(--font-size-sm);
		display: block;
		margin-top: var(--spacing-sm);
	}

	/* Venue */
	.venue-container {
		display: grid;
		grid-template-columns: 1fr 350px;
		gap: var(--spacing-2xl);
		align-items: start;
	}

	.venue-accordion {
		display: flex;
		flex-direction: column;
		gap: var(--spacing-sm);
	}

	.venue-accordion-item {
		background: var(--white);
		border: 1px solid var(--light-gray);
		border-radius: var(--radius-sm);
		overflow: hidden;
	}

	.venue-accordion-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: var(--spacing-md) var(--spacing-lg);
		cursor: pointer;
		transition: background 0.2s ease;
	}

	.venue-accordion-header:hover {
		background: var(--gray-50);
	}

	.venue-accordion-header h3 {
		font-size: var(--font-size-base);
		color: var(--text-dark);
		margin: 0;
	}

	.venue-accordion-content {
		padding: 0 var(--spacing-lg) var(--spacing-md);
	}

	.venue-accordion-content p,
	.venue-accordion-content li {
		line-height: 1.6;
		color: var(--text-dark);
		font-size: var(--font-size-sm);
	}

	.venue-accordion-content ul {
		margin-left: var(--spacing-lg);
	}

	/* Speakers */
	.speakers-slider {
		margin-top: var(--spacing-xl);
		display: grid;
		grid-auto-flow: column;
		overflow-x: scroll;
		scroll-snap-type: x mandatory;
		scroll-behavior: smooth;
		scrollbar-width: none;
	}

	.speakers-slider::-webkit-scrollbar {
		display: none;
	}

	.speakers-grid {
		scroll-snap-align: start;
		width: 100vw;
		max-width: 1400px;
		margin: 0 auto;
		display: flex;
		height: 600px;
		gap: 1.5rem;
		padding: 0 var(--spacing-xl);
	}

	.speaker-card {
		flex: 1 1 0;
		min-width: 0;
		position: relative;
		height: 100%;
		background-size: cover;
		background-position: center;
		border-radius: var(--radius-md);
		overflow: hidden;
		cursor: pointer;
		display: flex;
		align-items: flex-end;
		transition: flex-grow 0.3s ease;
	}

	.speaker-card::before {
		content: '';
		position: absolute;
		inset: 0;
		background: linear-gradient(180deg, transparent 40%, rgba(0,0,0,0.8) 100%);
	}

	.speaker-card:hover {
		flex-grow: 2;
	}

	.speaker-content {
		position: relative;
		z-index: 1;
		padding: var(--spacing-lg);
		width: 100%;
		color: var(--white);
	}

	.speaker-name {
		font-size: var(--font-size-base);
		font-weight: 700;
		margin-bottom: var(--spacing-xs);
		text-shadow: 0 1px 3px rgba(0,0,0,0.5);
	}

	.speaker-bio-preview {
		font-size: var(--font-size-sm);
		color: rgba(255, 255, 255, 0.9);
		line-height: 1.4;
		margin-bottom: var(--spacing-sm);
		display: -webkit-box;
		-webkit-line-clamp: 2;
		line-clamp: 2;
		-webkit-box-orient: vertical;
		overflow: hidden;
		opacity: 0;
		max-height: 0;
		transition: all 0.3s ease;
	}

	.speaker-card:hover .speaker-bio-preview {
		opacity: 1;
		max-height: 80px;
	}

	.view-bio-btn {
		color: var(--white);
		font-size: var(--font-size-sm);
		font-weight: 600;
		padding: var(--spacing-xs) var(--spacing-md);
		min-height: 44px;
		display: inline-flex;
		align-items: center;
		background: rgba(255, 255, 255, 0.2);
		border-radius: var(--radius-sm);
		border: 1px solid rgba(255, 255, 255, 0.3);
		transition: all 0.2s ease;
	}

	.view-bio-btn:hover {
		background: var(--white);
		color: var(--primary-blue);
	}

	.speakers-pager {
		display: flex;
		justify-content: center;
		gap: var(--spacing-md);
		margin-top: var(--spacing-lg);
	}

	.speakers-pager-btn {
		background: var(--white);
		border: 2px solid var(--primary-blue);
		color: var(--primary-blue);
	}

	/* Speaker Modal */
	.speaker-bio-modal {
		position: fixed;
		inset: 0;
		background: rgba(0, 0, 0, 0.8);
		z-index: 9999;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: var(--spacing-lg);
		overflow-y: auto;
	}

	.speaker-bio-content {
		background: var(--white);
		border-radius: var(--radius-md);
		max-width: 900px;
		width: 100%;
		max-height: 90vh;
		overflow-y: auto;
	}

	.speaker-bio-header {
		position: sticky;
		top: 0;
		background: var(--gray-50);
		padding: var(--spacing-lg);
		border-bottom: 2px solid var(--primary-orange);
		display: flex;
		gap: var(--spacing-lg);
		align-items: flex-start;
	}

	.speaker-bio-image {
		flex-shrink: 0;
		width: 100px;
		height: 100px;
		border-radius: 50%;
		overflow: hidden;
		background: linear-gradient(135deg, var(--primary-blue), var(--secondary-blue));
		display: flex;
		align-items: center;
		justify-content: center;
		border: 3px solid var(--white);
	}

	.speaker-bio-image img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.speaker-initials {
		font-size: 40px;
		font-weight: 700;
		color: var(--white);
	}

	.speaker-bio-info {
		flex: 1;
	}

	.speaker-bio-info h3 {
		font-size: var(--font-size-xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-xs);
	}

	.speaker-bio-info h4 {
		font-size: var(--font-size-sm);
		color: var(--medium-gray);
		font-weight: 500;
		line-height: 1.5;
	}

	.speaker-bio-body {
		padding: var(--spacing-lg);
	}

	.speaker-bio-body p {
		line-height: 1.7;
		color: var(--text-dark);
		margin-bottom: var(--spacing-md);
	}

	/* Responsive */
	@media (max-width: 768px) {
		.about-section,
		.video-section,
		.programme-intro,
		.venue-section,
		.speakers-intro,
		.hero-banner .container {
			padding: 0 var(--spacing-md);
		}

		.hero-banner h1 {
			font-size: var(--font-size-2xl);
		}

		.hero-actions {
			flex-direction: column;
			align-items: center;
		}

		.btn-hero {
			width: 100%;
			max-width: 280px;
			text-align: center;
		}

		.venue-container {
			grid-template-columns: 1fr;
		}

		.speakers-grid {
			height: 450px;
			gap: 1rem;
			padding: 0 var(--spacing-md);
		}

		.speaker-bio-header {
			flex-wrap: wrap;
		}

		.close-modal-btn {
			position: absolute;
			top: var(--spacing-sm);
			right: var(--spacing-sm);
		}

		.expect-grid {
			grid-template-columns: repeat(2, 1fr);
		}

		.session-header {
			flex-direction: column;
		}

		.expand-icon {
			align-self: flex-end;
		}
	}

	@media (max-width: 480px) {
		.speakers-grid {
			height: 380px;
		}

		.speaker-bio-image {
			width: 80px;
			height: 80px;
		}

		.speaker-initials {
			font-size: 32px;
		}

		.expect-grid {
			grid-template-columns: 1fr;
		}
	}
</style>

<!-- Hero Banner -->
<div class="hero-banner">
	<div class="container">
		<h1>OHDSI Africa Symposium 2025: Building Collaborative Research Networks</h1>
		<p class="hero-date-location">
			November 10-12, 2025 • Kampala, Uganda
		</p>
		<div class="hero-actions">
			<a href="https://forms.office.com/pages/responsepage.aspx?id=4p--5enagUK1xV7iEx_EsW4qH1WABP9OiVTbq64DnSVUOEFRODJTRTQyQ0RVOFZNUEhFR09CTzRZNi4u&route=shorturl" class="btn-hero" target="_blank" rel="noopener noreferrer">
				Register to the Symposium
			</a>
			<a href="#programme" class="btn-hero">
				Programme
			</a>
			<a href="#speakers" class="btn-hero">
				Speakers
			</a>
			<a href="#venue" class="btn-hero">
				Venue & Travel
			</a>
		</div>
	</div>
</div>

<!-- About Section -->
<section class="section">
	<div class="container">
		<div class="about-section">
			<h2>About the Symposium</h2>
			<p>
				The inaugural OHDSI Africa Symposium is a landmark event that brings together researchers, clinicians, data scientists, and healthcare leaders at the forefront of observational health data research across Africa. This historic gathering will be held in Kampala at the Joint Clinical Research Centre (JCRC) and Mestil Hotel.
			</p>
			<p>
				Our community is delighted to introduce this new face-to-face opportunity in Africa, where OHDSI is growing at an exciting pace. The symposium is designed for inclusive participation and offers a unique platform for showcasing how African health data can be standardized and leveraged for high-quality research and policy.
			</p>
		</div>
	</div>
</section>

<!-- What to Expect -->
<section class="section bg-light">
	<div class="container">
		<h2 class="section-title">What to Expect</h2>
		<div class="expect-grid">
			<div class="expect-item">
				<div class="expect-number">3 Days</div>
				<div class="expect-label">Tutorial + Main Conference</div>
			</div>
			<div class="expect-item">
				<div class="expect-number">2 Venues</div>
				<div class="expect-label">JCRC & Mestil Hotel</div>
			</div>
			<div class="expect-item">
				<div class="expect-number">Multiple Sessions</div>
				<div class="expect-label">Tutorials • Talks • Posters</div>
			</div>
			<div class="expect-item">
				<div class="expect-number">Networking</div>
				<div class="expect-label">In-person Collaboration</div>
			</div>
		</div>
	</div>
</section>

<!-- Programme Section -->
<section class="section" id="programme">
	<div class="container">
		<div class="programme-intro">
			<h2>Our Programme at a Glance</h2>
			<p>
				The inaugural OHDSI Africa Symposium will feature a dedicated one-day training course at JCRC, followed by a two-day main conference at Mestil Hotel. The symposium will showcase real-world examples of OHDSI's impact on health outcomes and build capacity for African institutions to participate in global research networks.
			</p>
		</div>

		<!-- Session Cards -->
		<div style="max-width: 1200px; margin: 0 auto; padding: 0 var(--spacing-xl);">
			<!-- Tutorial Day -->
			<div class="session-card">
				<div class="session-header" on:click={() => toggleSection('tutorial')} on:keypress={(e) => e.key === 'Enter' && toggleSection('tutorial')} role="button" tabindex="0">
					<div class="session-title-group">
						<h3>Day 1: Tutorial Workshop</h3>
						<p class="session-meta">November 10, 2025 | JCRC</p>
						<span class="session-type">Hands-On Training</span>
					</div>
					<span class="expand-icon" class:expanded={expandedSection === 'tutorial'}>+</span>
				</div>
				{#if expandedSection === 'tutorial'}
					<div class="session-content">
						<p>
							The tutorial day provides hands-on training for participants to learn OHDSI tools and methodologies. This full-day workshop is designed for both newcomers and experienced users looking to deepen their understanding of the OHDSI ecosystem.
						</p>
						
						<h4 style="margin-top: var(--spacing-xl); margin-bottom: var(--spacing-md); color: var(--primary-blue); font-weight: 700;">Training Modules:</h4>
						
						<div class="training-module">
							<h5>1. OHDSI / OMOP Introduction</h5>
							<p>Participants will be introduced to the Observational Health Data Sciences and Informatics (OHDSI) community and the OMOP Common Data Model (CDM). They will learn about OHDSI's global mission to generate real-world evidence through standardized data and open-source analytics.</p>
							<p><strong>Learning Objectives:</strong></p>
							<ul>
								<li>Understand the vision, structure, and collaborative nature of OHDSI</li>
								<li>Recognize the role of the OMOP CDM in harmonizing health data for large-scale analysis</li>
								<li>Appreciate how OHDSI tools and methods support data-driven healthcare research</li>
				</ul>
							<p><em>Trainers: Michael O, Mui Van Zandt, Cynthia S</em></p>
			</div>

						<div class="training-module">
							<h5>2. OMOP CDM and Vocabulary</h5>
							<p>This session focuses on the structure and logic of the OMOP Common Data Model and its standardized vocabulary system. Participants will explore how data from diverse sources (EHRs, claims, registries) can be mapped into a common structure.</p>
							<p><strong>Learning Objectives:</strong></p>
							<ul>
								<li>Understand the key tables and relationships within the OMOP CDM</li>
								<li>Learn about standard vocabularies (SNOMED, RxNorm, LOINC, etc.) and their role in ensuring data interoperability</li>
								<li>Gain insights into how local terms and codes are mapped to OMOP standards</li>
				</ul>
							<p><em>Trainers: Sebastian Van Sandijk, Cynthia S, Mui Van Zandt</em></p>
			</div>

						<div class="training-module">
							<h5>3. OMOP Conversion Process</h5>
							<p>In this module, participants will learn the step-by-step process of transforming local datasets into the OMOP CDM. This includes understanding the planning, mapping, and transformation stages required for conversion.</p>
							<p><strong>Learning Objectives:</strong></p>
							<ul>
								<li>Be familiar with the workflow of an OMOP conversion project</li>
								<li>Identify the main challenges in data mapping and how to address them</li>
								<li>Understand the tools and resources available to support conversion efforts</li>
				</ul>
							<p><em>Trainers: Rachel O, Freija D., Ousmane D</em></p>
			</div>

						<div class="training-module">
							<h5>4. ETL Exercises</h5>
							<p>The Extract, Transform, Load (ETL) session offers hands-on experience in converting sample data into the OMOP CDM format. Participants will use practical examples to apply concepts learned in earlier sessions.</p>
							<p><strong>Learning Objectives:</strong></p>
							<ul>
								<li>Perform basic ETL operations on sample health datasets</li>
								<li>Understand data transformation logic and validation steps</li>
								<li>Gain practical skills in structuring data to meet OMOP CDM requirements</li>
							</ul>
							<p><em>Trainers: Pauline A, Freija D., Reinpeter M</em></p>
		</div>

						<div class="training-module">
							<h5>5. Data Quality Dashboard</h5>
							<p>Participants will be introduced to the Data Quality Dashboard (DQD) — an OHDSI tool used to assess and validate the quality of OMOP-converted data.</p>
							<p><strong>Learning Objectives:</strong></p>
							<ul>
								<li>Understand how the DQD checks data conformance, completeness, and plausibility</li>
								<li>Learn how to interpret DQD results and identify areas for improvement</li>
								<li>Appreciate the importance of maintaining high-quality standardized data for research</li>
							</ul>
							<p><em>Trainers: Reinpeter M, David A, Sebastian Van Sandijk</em></p>
		</div>

						<div class="training-module">
							<h5>6. Evidence Generation via OHDSI Tools</h5>
							<p>The final session will demonstrate how OHDSI's analytical tools are used to generate real-world evidence from standardized data. Participants will explore applications such as Atlas and ACHILLES.</p>
							<p><strong>Learning Objectives:</strong></p>
							<ul>
								<li>Understand the workflow from data to evidence generation within the OHDSI framework</li>
								<li>Learn how to use OHDSI tools to design studies, run analyses, and visualize results</li>
								<li>Appreciate how data standardization enables cross-country collaboration and reproducible research</li>
							</ul>
							<p><em>Trainers: Edward, Anna, Mui Van Zandt</em></p>
						</div>
					</div>
				{/if}
		</div>

			<!-- Main Conference Days -->
			<div class="session-card">
				<div class="session-header" on:click={() => toggleSection('conference')} on:keypress={(e) => e.key === 'Enter' && toggleSection('conference')} role="button" tabindex="0">
					<div class="session-title-group">
						<h3>Days 2-3: Main Conference</h3>
						<p class="session-meta">November 11-12, 2025 | Mestil Hotel</p>
						<span class="session-type">Keynotes • Panels • Showcases</span>
					</div>
					<span class="expand-icon" class:expanded={expandedSection === 'conference'}>+</span>
				</div>
				{#if expandedSection === 'conference'}
					<div class="session-content">
						<p>
							The two-day main conference features keynote addresses, panel discussions, and the Collaborator Showcase highlighting cutting-edge research using OHDSI tools across Africa.
						</p>
						<p><strong>Conference highlights:</strong></p>
						<ul>
							<li>Keynote presentations from OHDSI community leaders</li>
							<li>Lightning talks and poster presentations</li>
							<li>Panel discussions on African health data challenges and opportunities</li>
							<li>Networking sessions for collaborative partnerships</li>
							<li>Case studies demonstrating real-world OHDSI applications</li>
						</ul>
						<p style="margin-top: var(--spacing-lg); font-style: italic;">
							The detailed agenda will be shared when available. Please stay tuned for updates as the programme is finalized over the coming months.
						</p>
					</div>
				{/if}
		</div>

			<!-- Collaborator Showcase -->
			<div class="session-card">
				<div class="session-header" on:click={() => toggleSection('showcase')} on:keypress={(e) => e.key === 'Enter' && toggleSection('showcase')} role="button" tabindex="0">
					<div class="session-title-group">
						<h3>Collaborator Showcase</h3>
						<p class="session-meta">Lightning Talks & Poster Presentations</p>
						<span class="session-type">Call for Submissions</span>
					</div>
					<span class="expand-icon" class:expanded={expandedSection === 'showcase'}>+</span>
				</div>
				{#if expandedSection === 'showcase'}
					<div class="session-content">
						<p>
							OHDSI's mission is to improve health by empowering a community to collaboratively generate evidence that promotes better health decisions and better care. The Collaborator Showcase will highlight how African researchers are using OHDSI tools to advance observational health research.
						</p>
						
						<h4 style="color: var(--primary-blue); margin-top: var(--spacing-xl); margin-bottom: var(--spacing-md); font-weight: 600;">Presentation Formats</h4>
						
						<p><strong>POSTER:</strong> A poster-board presentation with a static display summary of your latest research. Poster measurements should be horizontal-orientation with a maximum size of 48 inches long × 36 inches high. Please create a thin fabric or paper-type poster (no foam core).</p>
						
				<p><strong>LIGHTNING TALK:</strong> A 6-minute podium presentation to verbally share your story with the community.</p>
						
						<h4 style="color: var(--primary-blue); margin-top: var(--spacing-xl); margin-bottom: var(--spacing-md); font-weight: 600;">Topics</h4>
						<p>Submissions should align with at least one of OHDSI's strategic areas:</p>
						<ul>
					<li>Observational data standards and management</li>
					<li>Methodological research</li>
					<li>Open-source analytics development</li>
					<li>Clinical applications</li>
					<li>Community development</li>
							<li>Clinical characterization, population-level estimation, or patient-level prediction</li>
				</ul>
						
						<h4 style="color: var(--primary-blue); margin-top: var(--spacing-xl); margin-bottom: var(--spacing-md); font-weight: 600;">Submission Guidelines</h4>
						<ul>
							<li>Maximum 1,000 words (exclusive of references/authors/affiliations/tables/figures)</li>
							<li>Maximum 10 MB file size</li>
							<li>Clearly state names of all co-authors and affiliations</li>
							<li>Include sections: Background, Methods, Results, Conclusion</li>
							<li>References recommended (maximum 15)</li>
							<li>Must be unpublished in peer-review literature (preprints acceptable)</li>
				</ul>
						
						<p style="margin-top: var(--spacing-lg);">
							For questions about submissions, please contact <a href="mailto:ohdsi@jcrc.org.ug">ohdsi@jcrc.org.ug</a>
						</p>
			</div>
				{/if}
		</div>
		</div>
	</div>
</section>

<!-- Speakers Section -->
<section class="section bg-light" id="speakers">
	<div class="speakers-intro">
		<h2>Featured Speakers & Contributors</h2>
		<p>
			The inaugural OHDSI Africa Symposium brings together leading voices in observational health data sciences, digital health innovation, and public health from across Africa and around the world. Meet the experts who will be sharing their insights, experiences, and vision for the future of health data research.
		</p>
	</div>

	<!-- Speakers Slider -->
	<div class="speakers-slider">
		{#each slideSpeakers as slideGroup}
			<div class="speakers-grid">
				{#each slideGroup as speaker}
					<div 
						class="speaker-card" 
						style="background-image: url({speaker.image || 'data:image/svg+xml,%3Csvg xmlns=\'http://www.w3.org/2000/svg\' width=\'400\' height=\'600\'%3E%3Cdefs%3E%3ClinearGradient id=\'grad\' x1=\'0%25\' y1=\'0%25\' x2=\'100%25\' y2=\'100%25\'%3E%3Cstop offset=\'0%25\' style=\'stop-color:%234A90E2;stop-opacity:1\' /%3E%3Cstop offset=\'100%25\' style=\'stop-color:%232E5C8A;stop-opacity:1\' /%3E%3C/linearGradient%3E%3C/defs%3E%3Crect width=\'400\' height=\'600\' fill=\'url(%23grad)\' /%3E%3Ctext x=\'50%25\' y=\'50%25\' font-family=\'Arial, sans-serif\' font-size=\'120\' font-weight=\'bold\' fill=\'white\' text-anchor=\'middle\' dominant-baseline=\'middle\'%3E{speaker.initials}%3C/text%3E%3C/svg%3E'})"
						on:click={() => toggleSpeaker(speaker.id)} 
						on:keypress={(e) => e.key === 'Enter' && toggleSpeaker(speaker.id)} 
						role="button" 
						tabindex="0"
						aria-label="View bio for {speaker.name}"
					>
						<div class="speaker-content">
							<h4 class="speaker-name">{speaker.name}</h4>
							<p class="speaker-bio-preview">{speaker.title}</p>
							<span class="view-bio-btn">View Bio</span>
						</div>
					</div>
				{/each}
			</div>
		{/each}
	</div>

	<div class="container">
		<!-- Slide Indicators -->
		<div class="speakers-indicators">
			{#each Array(totalSlides) as _, index}
				<div 
					class="speaker-indicator" 
					class:active={currentSlide === index}
					on:click={() => goToSlide(index)}
					on:keypress={(e) => e.key === 'Enter' && goToSlide(index)}
					role="button"
					tabindex="0"
					aria-label="Go to slide {index + 1}"
				></div>
			{/each}
		</div>

		<!-- Navigation Buttons -->
		<div class="speakers-pager">
			<button 
				class="speakers-pager-btn" 
				on:click={prevSlide}
				disabled={currentSlide === 0}
				aria-label="Previous slide"
			>
				<svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
					<path d="M15 18L9 12L15 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
				</svg>
			</button>
			<button 
				class="speakers-pager-btn" 
				on:click={nextSlide}
				disabled={currentSlide === totalSlides - 1}
				aria-label="Next slide"
			>
				<svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
					<path d="M9 18L15 12L9 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
				</svg>
			</button>
		</div>
	</div>
</section>

<!-- Single Dynamic Speaker Bio Modal -->
{#if expandedSpeaker && currentSpeaker}
	<!-- svelte-ignore a11y_no_noninteractive_element_interactions -->
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keydown={(e) => e.key === 'Escape' && toggleSpeaker('')} role="dialog" tabindex="-1" aria-modal="true" aria-labelledby="speaker-modal-title">
		<!-- svelte-ignore a11y_click_events_have_key_events a11y_no_static_element_interactions -->
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()}>
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if currentSpeaker.image}
						<img loading="lazy" src={currentSpeaker.image} alt={currentSpeaker.name} />
					{:else}
						<div class="speaker-initials">{currentSpeaker.initials}</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3 id="speaker-modal-title">{currentSpeaker.name}</h3>
					<h4>{currentSpeaker.title}</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')} aria-label="Close">×</button>
			</div>
			<div class="speaker-bio-body">
				<p>{currentSpeaker.bio}</p>
			</div>
		</div>
	</div>
{/if}

<!-- Symposium Highlights Section -->
<section class="section bg-light">
	<div class="container">
		<div class="video-section">
			<h2 class="section-title">Symposium Highlights</h2>
			<p class="video-intro">
				Watch the official highlight reel from the OHDSI Africa Symposium 2025, featuring keynote presentations, 
				collaborative workshops, and memorable moments from Africa's premier health data science gathering.
			</p>
			<div class="video-wrapper">
				<iframe 
					src="https://www.youtube.com/embed/p1gAqFnBnvI" 
					title="OHDSI Africa Symposium 2025 - Symposium Highlights" 
					frameborder="0" 
					allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
					referrerpolicy="strict-origin-when-cross-origin" 
					allowfullscreen
					loading="lazy"
				></iframe>
			</div>
		</div>
	</div>
</section>



<!-- Venue & Travel Section -->
<section class="section" id="venue">
	<div class="venue-section">
		<div class="venue-container">
			<!-- Left: Accordion -->
			<div class="venue-accordion-wrapper">
				<h2>Venue & Travel Information</h2>
				<div class="venue-accordion">
				<!-- Event Venues -->
				<div class="venue-accordion-item">
					<div class="venue-accordion-header" on:click={() => toggleVenue('venues')} on:keypress={(e) => e.key === 'Enter' && toggleVenue('venues')} role="button" tabindex="0">
						<h3>Event Venues</h3>
						<span class="expand-icon" class:expanded={expandedVenue === 'venues'}>+</span>
					</div>
					{#if expandedVenue === 'venues'}
						<div class="venue-accordion-content">
							<p><strong>Pre-Symposium Training (November 10, 2025)</strong></p>
							<p><strong>Venue:</strong> Joint Clinical Research Centre (JCRC) Offices, Lubowa</p>
							<p><strong>Time:</strong> 8:30 AM – 5:00 PM</p>
							<p><strong>Facilities:</strong></p>
							<ul>
								<li>Wi-Fi available throughout</li>
								<li>Tea and lunch provided on-site</li>
								<li>Shuttle transportation arranged from Mestil Hotel for registered guests</li>
			</ul>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Main Symposium (November 11–12, 2025)</strong></p>
							<p><strong>Venue:</strong> Mestil Hotel & Residences, Nsambya, Kampala</p>
							<p><strong>Time:</strong> 8:30 AM – 4:30 PM (both days)</p>
							<p><strong>Facilities:</strong></p>
							<ul>
								<li>Plenary and breakout session rooms</li>
								<li>Exhibition area for partners and sponsors</li>
								<li>Complimentary tea, coffee, and lunch</li>
								<li>Networking dinner (details to follow)</li>
							</ul>
							
							<p style="margin-top: var(--spacing-md);"><strong>Note:</strong> JCRC is in Lubowa, along Entebbe Road, about 20-45 minutes from central Kampala hotels (depending on traffic).</p>
						</div>
					{/if}
				</div>

				<!-- Accommodations -->
				<div class="venue-accordion-item">
					<div class="venue-accordion-header" on:click={() => toggleVenue('accommodations')} on:keypress={(e) => e.key === 'Enter' && toggleVenue('accommodations')} role="button" tabindex="0">
						<h3>Accommodations</h3>
						<span class="expand-icon" class:expanded={expandedVenue === 'accommodations'}>+</span>
					</div>
					{#if expandedVenue === 'accommodations'}
						<div class="venue-accordion-content">
							<p><strong>Recommended Hotel: Mestil Hotel & Residences</strong></p>
							<p>Participants staying at Mestil enjoy proximity to the main symposium venue, high-speed internet, gym access, and breakfast.</p>
							<p><strong>Booking Code:</strong> JCRC</p>
							<p>
								<strong>Booking Link:</strong> 
								<a href="https://direct-book.com/properties/MestilDIRECT?promotion_code=JCRC25" target="_blank" rel="noopener noreferrer">
									Book Now at Mestil Hotel
								</a>
							</p>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Alternative Hotels:</strong></p>
							
							<p style="margin-top: var(--spacing-md);"><strong>Luxury / VIP Options:</strong></p>
							<ul>
								<li><strong>Kampala Serena Hotel</strong> - 5-star, very comfortable, central, excellent service</li>
								<li><strong>Sheraton Kampala Hotel (Marriott)</strong> - Upscale, international standard, business-friendly</li>
							</ul>
							
							<p style="margin-top: var(--spacing-md);"><strong>Mid-range / Business-Friendly:</strong></p>
							<ul>
								<li><strong>Speke Hotel</strong> - Historic and central, reliable, close to major city spots</li>
								<li><strong>Fairway Hotel</strong> - Convenient location</li>
								<li><strong>Hotel Africana</strong> - Good facilities</li>
								<li><strong>Hilton Garden Inn / Protea Hotel / Latitude 0 Degrees</strong> - Modern hotels offering good Wi-Fi and airport transfers</li>
							</ul>
							
							<p style="margin-top: var(--spacing-md);"><strong>Budget / Long-Stay Options:</strong></p>
							<ul>
								<li><strong>Arcadia Suites / Kampala Boulevard Suites</strong> - Serviced apartments, affordable</li>
							</ul>
							
							<p style="margin-top: var(--spacing-md);"><strong>Booking Tips:</strong> Prioritize hotels offering airport transfers, early breakfast, and strong Wi-Fi. Consider group booking arrangements for convenience and cost savings.</p>
						</div>
					{/if}
				</div>

				<!-- Transportation -->
				<div class="venue-accordion-item">
					<div class="venue-accordion-header" on:click={() => toggleVenue('transportation')} on:keypress={(e) => e.key === 'Enter' && toggleVenue('transportation')} role="button" tabindex="0">
						<h3>Transportation & Transit</h3>
						<span class="expand-icon" class:expanded={expandedVenue === 'transportation'}>+</span>
					</div>
					{#if expandedVenue === 'transportation'}
						<div class="venue-accordion-content">
							<p><strong>Airport Transportation</strong></p>
							<p>
								Kampala is served by <strong>Entebbe International Airport</strong>, located approximately 45 km from the city center and the symposium venues.
							</p>
							<p>
								Taxis are available at the airport—please use official airport taxis, which can be arranged at the airport help desk upon arrival. The journey to Kampala typically takes about 45–60 minutes, depending on traffic.
							</p>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Symposium Shuttle Service</strong></p>
							<p>
								Shuttle service between Mestil Hotel and JCRC will be provided during training day (November 10) for registered guests staying at Mestil Hotel.
							</p>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Ride-Hailing Apps</strong></p>
							<p>
								<a href="https://play.google.com/store/apps/details?id=com.safeboda.passenger&hl=en" target="_blank" rel="noopener noreferrer">Safe Boda</a>, <a href="https://play.google.com/store/apps/details?id=com.faras.rider&hl=en" target="_blank" rel="noopener noreferrer">Faras</a>, and Uber are widely available in Kampala for convenient transportation.
							</p>
							
							<p style="margin-top: var(--spacing-md);"><strong>Travel Tip:</strong> Confirm taxi fares before starting your journey.</p>
						</div>
					{/if}
		</div>

				<!-- General Information -->
				<div class="venue-accordion-item">
					<div class="venue-accordion-header" on:click={() => toggleVenue('general')} on:keypress={(e) => e.key === 'Enter' && toggleVenue('general')} role="button" tabindex="0">
						<h3>General Information</h3>
						<span class="expand-icon" class:expanded={expandedVenue === 'general'}>+</span>
					</div>
					{#if expandedVenue === 'general'}
						<div class="venue-accordion-content">
							<p><strong>Languages:</strong> English, Luganda</p>
							
							<p style="margin-top: var(--spacing-md);"><strong>Currency:</strong> Ugandan Shilling (UGX)</p>
							<ul>
								<li>US Dollar: 1 USD = 3,486 UGX (approx.)</li>
								<li>Euro: 1 EUR = 4,029 UGX (approx.)</li>
								<li>British Pound: 1 GBP = 4,572 UGX (approx.)</li>
								<li>Major credit cards accepted. ATMs available in shopping malls.</li>
			</ul>
							
							<p style="margin-top: var(--spacing-md);"><strong>Time Zone:</strong> East Africa Time (EAT, UTC+3)</p>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Weather (November)</strong></p>
							<ul>
								<li>Expect frequent afternoon or evening showers, sometimes heavy but brief</li>
								<li>Mornings are often clear, making them best for outdoor travel or photography</li>
								<li>Temperatures remain pleasant - usually 21-27°C</li>
								<li>Carry a light raincoat or umbrella</li>
							</ul>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Power & Electricity</strong></p>
							<p>
								Uganda uses Type G (three rectangular pins) and Type C (two round pins) power sockets. 
								The electricity supply operates at 230V and 50Hz. Most hotels and venues have universal sockets 
								that fit both plug types. International participants are advised to bring a universal travel adapter 
								to ensure compatibility with their devices.
							</p>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Health & Safety</strong></p>
							<p>Uganda requires a yellow fever vaccination certificate for international travelers. Ensure valid travel insurance and follow health advisories.</p>
							
							<p style="margin-top: var(--spacing-md);"><strong>Recommended Hospitals:</strong></p>
							<ul>
								<li><strong>St. Francis Hospital Nsambya</strong> - +256 41 4267012, +256 41 4266998</li>
								<li><strong>International Hospital Kampala (IHK)</strong> - +256 771 801902</li>
								<li><strong>Kibuli Muslim Hospital</strong> - +256 41 4236476/7</li>
								<li><strong>Nakasero Hospital</strong> - +256 393 224 681</li>
							</ul>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Tours & Cultural Experiences (Optional)</strong></p>
							<p>Participants interested in exploring Uganda can consider:</p>
							<ul>
								<li><strong>Kampala City Tour:</strong> Visit the Uganda Museum, Kasubi Tombs, and local craft markets</li>
								<li><strong>Source of the Nile (Jinja) Tour:</strong> Day trip to Jinja, Uganda's adventure capital</li>
								<li><strong>Wildlife Excursions:</strong> Short safaris to Lake Mburo or Murchison Falls National Park can be arranged through recommended tour operators</li>
							</ul>
							
							<p style="margin-top: var(--spacing-lg);"><strong>Contact for Logistical Inquiries:</strong></p>
							<p>For logistical inquiries or special requests, contact: <strong>+256 782 967063</strong></p>
							
							<p style="margin-top: var(--spacing-lg);">
								For non-OHDSI-related information, please visit the <a href="https://jcrc.org.ug" target="_blank" rel="noopener noreferrer">JCRC website</a>.
							</p>
						</div>
					{/if}
				</div>
				</div>
		</div>
		</div>
	</div>
</section>

