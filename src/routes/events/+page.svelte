<script lang="ts">
	import { onMount } from 'svelte';
	import PageHeader from '$lib/components/PageHeader.svelte';
	
	let expandedSection: string | null = null;
	let expandedSpeaker: string | null = null;
	let expandedVenue: string | null = null;
	let currentSlide = 0;
	const speakersPerSlide = 6;
	let previousFocusElement: HTMLElement | null = null;
	
	// Speaker data array
	const speakers = [
		{ id: 'ryan', initials: 'PR', name: 'Dr. Patrick Ryan', title: 'VP of Observational Health Data Analytics, Janssen R&D; OHDSI Founding Collaborator', image: '/speakers/patrick-ryan.jpg' },
		{ id: 'vanzandt', initials: 'MV', name: 'Mui Van Zandt', title: 'VP & Global Head of Data Strategy, IQVIA; Asia-Pacific OHDSI Chapter Lead', image: '/speakers/mui-van-zandt.jpg'},
		{ id: 'aceng', initials: 'JA', name: 'Dr. Jane Ruth Aceng', title: 'Minister of Health, Uganda', image: '/speakers/jane-aceng.jpg' },
		{ id: 'sung', initials: 'CS', name: 'Prof. Cynthia Sung, PhD', title: 'Adjunct Associate Professor, Duke-NUS; Co-Lead, OHDSI Africa Chapter', image: '/speakers/cynthia-sung.jpg' },
		{ id: 'kiragga', initials: 'AK', name: 'Dr. Agnes Kiragga', title: 'Data Science Program Lead, African Population Health Research Council', image: '/speakers/agnes-kiragga.jpg' },
		{ id: 'asiimwe', initials: 'AA', name: 'Alex Asiimwe', title: 'Head of Evidence Generation, Innovation, and Partnerships, Gilead Sciences', image: '/speakers/alex-asiimwe.jpg' },
		{ id: 'kanter', initials: 'AK', name: 'Dr. Andrew S. Kanter', title: 'Assistant Professor, Columbia University; Strategic Advisor, IMO Health', image: '/speakers/andrew-kanter.jpg' },
		{ id: 'muyingo', initials: 'SM', name: 'Dr. Sylvia Muyingo', title: 'Research Scientist, African Population and Health Research Centre (APHRC)', image: '/speakers/silvia-muyingo.jpg'},
		{ id: 'burn', initials: 'EB', name: 'Dr. Edward Burn', title: 'Senior Researcher, University of Oxford; DARWIN-EU & HERON-UK Projects', image: '/speakers/edward-burn.jpg' },
		{ id: 'ario', initials: 'AA', name: 'Prof. Dr. Alex Riolexus Ario', title: 'Program Director, Uganda Public Health Fellowship Program; Director, Uganda National Institute of Public Health',image: '/speakers/alex-ario.jpg' },
		{ id: 'herbst', initials: 'KH', name: 'Dr. Kobus Herbst', title: 'Director of Population Science, Africa Health Research Institute (AHRI)', image: '/speakers/kobus-herbst.jpg' },
		{ id: 'nakazibwe', initials: 'BN', name: 'Brenda Nakazibwe', title: 'Team Lead, Pathogen Economy, Science, Technology & Innovation Secretariat (STIS), Uganda', image: 'speakers/brenda-nakazibwe.webp' },
		{ id: 'ganda', initials: 'GG', name: 'Dr. Gregory Ganda', title: 'County Executive Committee Member for Health, Kisumu County, Kenya', image: '/speakers/gregory-ganda.jpg' },
		{ id: 'waynee', initials: 'SW', name: 'Steven Waynee', title: 'Founder & CEO, IntelliSOFT; President, HELINA; Secretary General, KeHIA', image: '/speakers/steven-waynee.jpg' },
		{ id: 'mbaka', initials: 'PM', name: 'Paul Mbaka', title: 'Assistant Commissioner, Health Information, Ministry of Health, Uganda', image: '/speakers/paul-mbaka.jpg' },
		{ id: 'bouras', initials: 'AB', name: 'Dr. Adam Bouras', title: 'Founder, Tritonis Inc.; Public Health Informatics Fellow, CDC', image: '/speakers/adam-bouras.jpg' },
		{ id: 'walravens', initials: 'MW', name: 'Michel Walravens', title: 'Rheumatologist & Researcher, Hasselt University; Developer, OikoLexis Platform', image: '/speakers/michel-walravens.jpg' },
		{ id: 'atwine', initials: 'MA', name: 'Mugume Atwine', title: 'Data Scientist, Uganda', image: '/speakers/mugume-atwine.jpg' },
		{ id: 'tamirat', initials: 'BT', name: 'Bekure Tamirat', title: 'Data Science & Analytics Unit Lead, Africa CDC', image: '/speakers/bekure-tamirat.jpg' },
		{ id: 'bagarukayo', initials: 'KB', name: 'Kenneth Bagarukayo', title: 'Commissioner ICT Research and Development, Ministry of ICT, Uganda', image: '/speakers/kenneth-bagarukayo.jpg' },
		{ id: 'kadengye', initials: 'DK', name: 'Dr. Damazo T. Kadengye', title: 'Head of Data, Measurement & Evaluation, African Population and Health Research Center', image: '/speakers/damazo-kadengye.webp' },
		{ id: 'tamale', initials: 'WT', name: 'Dr. William Tamale', title: 'Clinical Manager, Joint Clinical Research Centre (JCRC)', image: '/speakers/william-tamale.png' },
		{ id: 'taylor', initials: 'AT', name: 'Amelia Taylor', title: 'Research Fellow, University of Nottingham', image: '/speakers/amelia-taylor.png' },
		{ id: 'martufi', initials: 'VM', name: 'Valentina Martufi', title: 'Researcher, Fiocruz, Brazil', image: '/speakers/valentina-martufi.jpg' },
		{ id: 'weerasinghe', initials: 'JW', name: 'Jayasanka Weerasinghe', title: 'Product Quality Engineering Lead, OpenMRS', image: '/speakers/jayasanka-weerasinghe.jpg' },
		{ id: 'fankoua', initials: 'LF', name: 'Luc Baudoin Fankoua', title: 'Health Informatics & Data Science Professional, Douala General Hospital, Cameroon', image: '/speakers/luc-baudoin-fankoua.jpg' },
		{ id: 'saura', initials: 'AS', name: 'Dr. Anna Saura Lázaro', title: 'Senior Researcher, Clinical Epidemiology and RWE, University of Oxford', image: '/speakers/anna-saura-lazaro.webp' },
		{ id: 'belmans', initials: 'LB', name: 'Dr. Luc Belmans', title: 'CEO, Medaman', image: '/speakers/luc-belmans.jpg' },
		{ id: 'kanyike', initials: 'FK', name: 'Dr. Francis Kanyike', title: 'Public Health Specialist, Joint Clinical Research Centre (JCRC), Uganda', image: '/speakers/francis-kanyike.jpg' },
		{ id: 'descamps', initials: 'FD', name: 'Freija Descamps', title: 'Managing Partner, edenceHealth NV', image: '/speakers/freija-descamps.jpg' }
	];
	
	$: totalSlides = Math.ceil(speakers.length / speakersPerSlide);
	$: slideSpeakers = Array.from({ length: totalSlides }, (_, i) => 
		speakers.slice(i * speakersPerSlide, (i + 1) * speakersPerSlide)
	);
	
	function toggleSection(section: string) {
		expandedSection = expandedSection === section ? null : section;
	}

	function toggleSpeaker(speaker: string) {
		if (expandedSpeaker === speaker) {
			// Closing modal - return focus to trigger
			expandedSpeaker = null;
			if (previousFocusElement) {
				setTimeout(() => previousFocusElement?.focus(), 100);
				previousFocusElement = null;
			}
		} else {
			// Opening modal - save focus and focus close button
			previousFocusElement = document.activeElement as HTMLElement;
			expandedSpeaker = speaker;
			setTimeout(() => {
				const closeBtn = document.querySelector('.close-modal-btn') as HTMLElement;
				closeBtn?.focus();
			}, 100);
		}
	}

	// Handle Escape key to close modal
	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape' && expandedSpeaker) {
			toggleSpeaker(expandedSpeaker);
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
		const slider = document.querySelector('.speakers-slider');
		const grids = slider?.querySelectorAll('.speakers-grid');
		if (grids && grids[index]) {
			grids[index].scrollIntoView({ behavior: 'smooth', block: 'nearest', inline: 'start' });
		}
	}

	// Lock body scroll when modal is open
	$: if (expandedSpeaker) {
		document.body.style.overflow = 'hidden';
	} else {
		document.body.style.overflow = '';
	}
</script>

<svelte:window on:keydown={handleKeydown} />

<svelte:head>
	<title>2025 Africa Symposium - OHDSI Africa</title>
	<meta name="description" content="Join us for the inaugural OHDSI Africa Symposium, November 10-12, 2025 in Kampala, Uganda." />
</svelte:head>

<style>
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
		font-size: var(--font-size-4xl);
		font-weight: 700;
		margin-bottom: var(--spacing-lg);
		color: var(--white);
	}

	.hero-date-location {
		font-size: var(--font-size-xl);
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
		border-radius: var(--radius-lg);
		text-decoration: none;
		font-weight: 600;
		font-size: var(--font-size-base);
		transition: all 0.3s ease;
		display: inline-block;
		box-shadow: 0 4px 12px rgba(255, 107, 53, 0.3);
	}

	.btn-hero:hover {
		transform: translateY(-2px);
		box-shadow: 0 8px 20px rgba(255, 107, 53, 0.4);
		background-color: var(--dark-orange);
	}

	.about-section {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 var(--spacing-xl);
		text-align: center;
	}

	.about-section h2 {
		font-size: var(--font-size-3xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-lg);
		font-weight: 700;
		border-bottom: solid 2px var(--disabled);
		padding-bottom: var(--spacing-sm);
	}

	.about-section p {
		font-size: var(--font-size-lg);
		line-height: 1.8;
		margin-bottom: var(--spacing-lg);
		color: var(--text-dark);
	}

	/* OHDSI-style section titles */
	.section-title {
		font-size: var(--font-size-3xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-2xl);
		font-weight: 700;
		border-bottom: solid 2px var(--disabled);
		padding-bottom: var(--spacing-sm);
		text-align: center;
	}

	.expect-grid {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
		gap: var(--spacing-xl);
		margin-top: var(--spacing-2xl);
	}

	.expect-item {
		text-align: center;
		padding: var(--spacing-xl);
		background: var(--white);
		border-radius: var(--radius-lg);
		box-shadow: var(--shadow-md);
	}

	.expect-number {
		font-size: var(--font-size-4xl);
		font-weight: 700;
		color: var(--primary-orange);
		margin-bottom: var(--spacing-sm);
	}

	.expect-label {
		font-size: var(--font-size-base);
		color: var(--text-dark);
		font-weight: 500;
	}

	.programme-intro {
		max-width: 1200px;
		margin: 0 auto var(--spacing-2xl);
		padding: 0 var(--spacing-xl);
		text-align: center;
	}

	.programme-intro h2 {
		font-size: var(--font-size-3xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-lg);
		font-weight: 700;
		border-bottom: solid 2px var(--disabled);
		padding-bottom: var(--spacing-sm);
	}

	.programme-intro p {
		font-size: var(--font-size-lg);
		line-height: 1.8;
		color: var(--text-dark);
	}

	.session-card {
		background: var(--white);
		border-radius: var(--radius-lg);
		margin-bottom: var(--spacing-lg);
		box-shadow: var(--shadow-md);
		overflow: hidden;
		transition: all 0.3s ease;
	}

	.session-card:hover {
		box-shadow: var(--shadow-lg);
		border-left: 4px solid var(--primary-orange);
	}

	.session-header {
		padding: var(--spacing-xl);
		cursor: pointer;
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		background: var(--light-blue);
		transition: background 0.3s ease;
	}

	.session-header:hover {
		background: var(--light-gray);
	}

	.session-title-group h3 {
		font-size: var(--font-size-xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-sm);
		font-weight: 600;
	}

	.session-meta {
		font-size: var(--font-size-sm);
		color: var(--medium-gray);
		margin-bottom: var(--spacing-sm);
	}

	.session-type {
		display: inline-block;
		background: var(--primary-orange);
		color: var(--white);
		padding: var(--spacing-xs) var(--spacing-md);
		border-radius: var(--radius-md);
		font-size: var(--font-size-sm);
		font-weight: 600;
	}

	.expand-icon {
		font-size: var(--font-size-2xl);
		color: var(--primary-blue);
		transition: transform 0.3s ease;
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
		padding: var(--spacing-xl);
		border-top: 1px solid var(--light-gray);
	}

	.session-content p {
		line-height: 1.8;
		margin-bottom: var(--spacing-lg);
		color: var(--text-dark);
	}

	.session-content ul {
		margin-left: var(--spacing-xl);
		margin-bottom: var(--spacing-lg);
	}

	.session-content li {
		line-height: 1.8;
		margin-bottom: var(--spacing-sm);
		color: var(--text-dark);
	}

	.session-content a {
		color: var(--secondary-blue);
		text-decoration: none;
		font-weight: 500;
		border-bottom: 1px solid transparent;
		transition: border-color 0.3s ease;
	}

	.session-content a:hover {
		border-bottom-color: var(--secondary-blue);
	}

	.training-module {
		background: var(--light-blue);
		border-left: 4px solid var(--primary-orange);
		padding: var(--spacing-lg);
		margin-bottom: var(--spacing-xl);
		border-radius: var(--radius-md);
		transition: all 0.3s ease;
	}

	.training-module:hover {
		background: #e8f2fc;
		transform: translateX(4px);
		box-shadow: 0 4px 12px rgba(0,0,0,0.08);
	}

	.training-module h5 {
		color: var(--primary-blue);
		font-size: var(--font-size-lg);
		font-weight: 700;
		margin-bottom: var(--spacing-md);
	}

	.training-module p {
		margin-bottom: var(--spacing-md);
	}

	.training-module ul {
		margin-left: var(--spacing-lg);
		margin-bottom: var(--spacing-md);
	}

	.training-module ul li {
		margin-bottom: var(--spacing-sm);
		line-height: 1.6;
		color: var(--text-dark);
	}

	.training-module em {
		color: var(--medium-gray);
		font-size: var(--font-size-sm);
		display: block;
		margin-top: var(--spacing-md);
	}

	.venue-section {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 var(--spacing-xl);
	}

	.venue-container {
		max-width: 1200px;
		margin: 0 auto;
		padding: 0 var(--spacing-xl);
		display: grid;
		grid-template-columns: 1fr 400px;
		gap: var(--spacing-3xl);
		align-items: start;
	}

	.venue-accordion-wrapper h2 {
		font-size: var(--font-size-3xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-xl);
		font-weight: 700;
		border-bottom: solid 2px var(--disabled);
		padding-bottom: var(--spacing-sm);
	}

	.venue-accordion {
		display: flex;
		flex-direction: column;
		gap: var(--spacing-md);
	}

	.venue-accordion-item {
		background: var(--white);
		border: 1px solid var(--light-gray);
		border-radius: var(--radius-md);
		overflow: hidden;
		transition: all 0.3s ease;
	}

	.venue-accordion-item:hover {
		box-shadow: 0 2px 8px rgba(0,0,0,0.08);
	}

	.venue-accordion-header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: var(--spacing-lg) var(--spacing-xl);
		cursor: pointer;
		background: var(--white);
		transition: background 0.3s ease;
	}

	.venue-accordion-header:hover {
		background: var(--light-blue);
		border-left: 3px solid var(--primary-orange);
	}

	.venue-accordion-header h3 {
		font-size: var(--font-size-lg);
		font-weight: 600;
		color: var(--text-dark);
		margin: 0;
	}

	.venue-accordion-content {
		padding: 0 var(--spacing-xl) var(--spacing-lg) var(--spacing-xl);
		background: var(--white);
	}

	.venue-accordion-content p {
		line-height: 1.6;
		margin-bottom: var(--spacing-sm);
		color: var(--text-dark);
		font-size: var(--font-size-base);
	}

	.venue-accordion-content ul {
		margin-left: var(--spacing-lg);
		margin-bottom: var(--spacing-md);
	}

	.venue-accordion-content li {
		line-height: 1.6;
		margin-bottom: var(--spacing-xs);
		color: var(--text-dark);
		font-size: var(--font-size-base);
	}

	.venue-accordion-content a {
		color: var(--secondary-blue);
		text-decoration: none;
		font-weight: 500;
		border-bottom: 1px solid transparent;
		transition: border-color 0.3s ease;
	}

	.venue-accordion-content a:hover {
		border-bottom-color: var(--secondary-blue);
	}

	.venue-image-container {
		position: sticky;
		top: var(--spacing-2xl);
		border-radius: var(--radius-lg);
		overflow: hidden;
		box-shadow: var(--shadow-xl);
		border: 3px solid var(--primary-orange);
		position: relative;
		height: 100%;
		min-height: 600px;
	}

	.venue-image {
		width: 100%;
		height: 100%;
		display: block;
		object-fit: cover;
	}

	.venue-image-caption {
		background: linear-gradient(to top, rgba(0, 49, 66, 0.95) 0%, rgba(0, 49, 66, 0.8) 100%);
		padding: var(--spacing-md) var(--spacing-lg);
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
	}

	.venue-image-caption p {
		color: var(--white);
		font-size: var(--font-size-lg);
		font-weight: 600;
		margin: 0;
		text-align: center;
	}


	/* Speakers Section */
	.speakers-intro {
		max-width: 1200px;
		margin: 0 auto var(--spacing-2xl);
		padding: 0 var(--spacing-xl);
		text-align: center;
	}

	.speakers-intro h2 {
		font-size: var(--font-size-3xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-lg);
		font-weight: 700;
		border-bottom: solid 2px var(--disabled);
		padding-bottom: var(--spacing-sm);
	}

	.speakers-intro p {
		font-size: var(--font-size-lg);
		line-height: 1.8;
		color: var(--text-dark);
	}

	.speakers-slider {
		position: relative;
		margin-top: var(--spacing-2xl);
		display: grid;
		grid-auto-flow: column;
		overflow-x: scroll;
		scroll-snap-type: x mandatory;
		-webkit-overflow-scrolling: touch;
		scroll-behavior: smooth;
		width: 100%;
		scrollbar-width: none;
	}

	.speakers-slider::-webkit-scrollbar {
		width: 0;
		height: 0;
		display: none;
		background: transparent;
	}

	.speakers-grid {
		scroll-snap-align: start;
		width: 100vw;
		max-width: 1440px;
		margin: 0 auto;
		display: flex;
		align-items: center;
		height: 680px;
		column-gap: 2rem;
		padding: 0 var(--spacing-2xl);
	}

	.speaker-card {
		flex: 1 1 0;
		min-width: 0;
		position: relative;
		height: 100%;
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
		border-radius: var(--radius-lg);
		overflow: hidden;
		cursor: pointer;
		display: flex;
		align-items: flex-end;
		transition: .25s ease-in-out;
		backface-visibility: hidden;
		perspective: 1000px;
		z-index: 1;
	}

	.speaker-card::before {
		content: '';
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: linear-gradient(180deg, rgba(0,0,0,0) 0%, rgba(0,0,0,0.7) 100%);
		transition: all 0.3s ease;
	}

	.speaker-card:hover::before {
		background: linear-gradient(180deg, rgba(0,0,0,0.2) 0%, rgba(0,0,0,0.75) 100%);
	}

	.speaker-card:hover {
		flex-grow: 2;
		transform: translateY(-10px);
		box-shadow: 0 25px 60px rgba(0,0,0,0.4);
		z-index: 100;
		border-radius: var(--radius-xl);
	}

	.speaker-content {
		position: relative;
		z-index: 1;
		padding: var(--spacing-xl);
		width: 100%;
		color: var(--white);
		transform: translateY(0);
		transition: all 0.3s ease;
	}

	.speaker-card:hover .speaker-content {
		transform: translateY(-5px);
	}

	.speaker-name {
		font-size: var(--font-size-lg);
		font-weight: 700;
		color: var(--white);
		margin-bottom: var(--spacing-sm);
		line-height: 1.2;
		text-shadow: 0 2px 4px rgba(0,0,0,0.3);
	}

	.speaker-bio-preview {
		font-size: var(--font-size-sm);
		color: rgba(255, 255, 255, 0.9);
		line-height: 1.4;
		margin-bottom: var(--spacing-md);
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
		max-height: 100px;
	}

	/* Skeleton Loader - Available for dynamic loading states */
	@keyframes skeleton-loading {
		0% {
			background-position: -200px 0;
		}
		100% {
			background-position: calc(200px + 100%) 0;
		}
	}

	:global(.skeleton) {
		background: linear-gradient(
			90deg,
			#f0f0f0 0px,
			#f8f8f8 40px,
			#f0f0f0 80px
		);
		background-size: 200px 100%;
		animation: skeleton-loading 1.5s ease-in-out infinite;
	}

	:global(.skeleton-card) {
		flex: 1 1 0;
		min-width: 0;
		height: 100%;
		border-radius: var(--radius-lg);
		background: var(--light-gray);
		position: relative;
		overflow: hidden;
	}

	:global(.skeleton-content) {
		position: absolute;
		bottom: var(--spacing-xl);
		left: var(--spacing-xl);
		right: var(--spacing-xl);
	}

	:global(.skeleton-name) {
		height: 24px;
		width: 70%;
		background: rgba(255, 255, 255, 0.3);
		border-radius: var(--radius-sm);
		margin-bottom: var(--spacing-sm);
	}

	:global(.skeleton-title) {
		height: 16px;
		width: 90%;
		background: rgba(255, 255, 255, 0.2);
		border-radius: var(--radius-sm);
	}

	.view-bio-btn {
		display: inline-block;
		color: var(--white);
		font-size: var(--font-size-sm);
		font-weight: 600;
		text-decoration: none;
		padding: var(--spacing-sm) var(--spacing-md);
		min-height: 44px;
		display: inline-flex;
		align-items: center;
		background: rgba(255, 255, 255, 0.2);
		backdrop-filter: blur(10px);
		border-radius: var(--radius-md);
		border: 1px solid rgba(255, 255, 255, 0.3);
		transition: all 0.3s ease;
	}

	.view-bio-btn:hover {
		background: var(--white);
		color: var(--primary-blue);
		border-color: var(--white);
	}

	/* Speaker Bio Modal/Expanded View */
	.speaker-bio-modal {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		background: rgba(0, 0, 0, 0.8);
		z-index: 9999;
		display: flex;
		align-items: center;
		justify-content: center;
		padding: var(--spacing-xl);
		overflow-y: auto;
	}

	.speaker-bio-content {
		background: var(--white);
		border-radius: var(--radius-lg);
		max-width: 1000px;
		width: 100%;
		max-height: 90vh;
		overflow-y: auto;
		position: relative;
	}

	.speaker-bio-header {
		position: sticky;
		top: 0;
		background: var(--light-blue);
		padding: var(--spacing-xl);
		border-bottom: 3px solid var(--primary-orange);
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		gap: var(--spacing-lg);
	}

	.speaker-bio-image {
		flex-shrink: 0;
		width: 120px;
		height: 120px;
		border-radius: 50%;
		overflow: hidden;
		background: linear-gradient(135deg, var(--primary-blue) 0%, var(--secondary-blue) 100%);
		display: flex;
		align-items: center;
		justify-content: center;
		border: 4px solid var(--white);
		box-shadow: 0 4px 12px rgba(0,0,0,0.15);
	}

	.speaker-bio-image img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.speaker-initials {
		font-size: 48px;
		font-weight: 700;
		color: var(--white);
	}

	.speaker-bio-info {
		flex: 1;
	}

	.speaker-bio-info h3 {
		font-size: var(--font-size-2xl);
		color: var(--primary-blue);
		margin-bottom: var(--spacing-sm);
		font-weight: 700;
	}

	.speaker-bio-info h4 {
		font-size: var(--font-size-base);
		color: var(--medium-gray);
		font-weight: 500;
		line-height: 1.6;
	}

	.close-modal-btn {
		background: var(--white);
		border: none;
		min-width: 44px;
		min-height: 44px;
		width: 44px;
		height: 44px;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		font-size: var(--font-size-2xl);
		color: var(--primary-blue);
		transition: all 0.3s ease;
		flex-shrink: 0;
	}

	.close-modal-btn:hover {
		background: var(--primary-orange);
		color: var(--white);
		transform: rotate(90deg);
	}

	.speaker-bio-body {
		padding: var(--spacing-xl);
	}

	.speaker-bio-body p {
		line-height: 1.8;
		color: var(--text-dark);
		margin-bottom: var(--spacing-lg);
	}

	/* Carousel Navigation */
	.speakers-indicators {
		display: flex;
		justify-content: center;
		gap: var(--spacing-sm);
		margin-top: var(--spacing-xl);
	}

	.speaker-indicator {
		width: 12px;
		height: 12px;
		border-radius: 50%;
		background: var(--light-gray);
		border: 2px solid var(--medium-gray);
		cursor: pointer;
		transition: all 0.3s ease;
	}

	.speaker-indicator.active {
		background: var(--primary-orange);
		border-color: var(--primary-orange);
	}

	.speakers-pager {
		display: flex;
		justify-content: center;
		gap: var(--spacing-lg);
		margin-top: var(--spacing-xl);
	}

	.speakers-pager-btn {
		width: 48px;
		height: 48px;
		border-radius: 50%;
		background: var(--white);
		border: 2px solid var(--primary-blue);
		color: var(--primary-blue);
		display: flex;
		align-items: center;
		justify-content: center;
		cursor: pointer;
		transition: all 0.3s ease;
		font-size: var(--font-size-xl);
	}

	.speakers-pager-btn:hover:not(:disabled) {
		background: var(--primary-orange);
		color: var(--white);
		transform: scale(1.1);
	}

	.speakers-pager-btn:disabled {
		opacity: 0.3;
		cursor: not-allowed;
		border-color: var(--medium-gray);
		color: var(--medium-gray);
	}

	@media (max-width: 768px) {
		.hero-banner .container,
		.about-section,
		.programme-intro,
		.venue-section,
		.speakers-intro {
			padding: 0 var(--spacing-lg);
		}

		.venue-container {
			grid-template-columns: 1fr;
			gap: var(--spacing-2xl);
			padding: 0 var(--spacing-lg);
		}

		.venue-image-container {
			position: relative;
			top: 0;
			min-height: 500px;
		}

		.venue-image {
			height: 100%;
		}

		.venue-accordion-header {
			padding: var(--spacing-md) var(--spacing-lg);
		}

		.venue-accordion-header h3 {
			font-size: var(--font-size-base);
		}

		.venue-accordion-content {
			padding: 0 var(--spacing-lg) var(--spacing-md) var(--spacing-lg);
		}

		.hero-banner h1 {
			font-size: var(--font-size-2xl);
		}

		.hero-date-location {
			font-size: var(--font-size-base);
		}

		.hero-actions {
			flex-direction: column;
			align-items: center;
		}

		.btn-hero {
			width: 100%;
			max-width: 300px;
		}

		.expect-grid {
			grid-template-columns: 1fr;
		}

		.session-header {
			flex-direction: column;
		}

		.expand-icon {
			align-self: flex-end;
		}

		.speakers-grid {
			height: 500px;
			column-gap: 1.5rem;
			padding: 0 var(--spacing-md);
		}

		.speaker-card:hover {
			flex-grow: 1.8;
			transform: translateY(-8px);
		}

		.speaker-name {
			font-size: var(--font-size-base);
		}

		.speaker-bio-modal {
			padding: var(--spacing-md);
		}

		.speaker-bio-header {
			flex-wrap: wrap;
		}

		.speaker-bio-image {
			width: 100px;
			height: 100px;
		}

		.speaker-initials {
			font-size: 40px;
		}

		.close-modal-btn {
			position: absolute;
			top: var(--spacing-md);
			right: var(--spacing-md);
		}

		.speakers-pager-btn {
			min-width: 44px;
			min-height: 44px;
			width: 44px;
			height: 44px;
		}
	}

	@media (max-width: 480px) {
		.hero-banner .container,
		.about-section,
		.programme-intro,
		.venue-section,
		.speakers-intro {
			padding: 0 var(--spacing-md);
		}

		.venue-container {
			padding: 0 var(--spacing-md);
		}

		.venue-image-container {
			position: relative;
			top: 0;
			min-height: 400px;
		}

		.venue-image {
			height: 100%;
		}

		.venue-image-caption p {
			font-size: var(--font-size-base);
		}

		.venue-accordion-header {
			padding: var(--spacing-sm) var(--spacing-md);
		}

		.venue-accordion-header h3 {
			font-size: var(--font-size-sm);
		}

		.venue-accordion-content {
			padding: 0 var(--spacing-md) var(--spacing-sm) var(--spacing-md);
			font-size: var(--font-size-sm);
		}

		.speakers-grid {
			height: 400px;
			column-gap: 1rem;
			padding: 0 var(--spacing-sm);
		}

		.speaker-card:hover {
			flex-grow: 1.5;
			transform: translateY(-6px);
		}

		.speaker-content {
			padding: var(--spacing-md);
		}

		.speaker-name {
			font-size: var(--font-size-sm);
		}

		.speaker-bio-preview {
			font-size: var(--font-size-xs);
		}

		.view-bio-btn {
			font-size: var(--font-size-xs);
			padding: var(--spacing-xs) var(--spacing-sm);
		}

		.speaker-bio-image {
			width: 80px;
			height: 80px;
		}

		.speaker-initials {
			font-size: 32px;
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

<!-- Speaker Bio Modals -->
{#if expandedSpeaker === 'ryan'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'ryan')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'ryan')?.image} alt="Dr. Patrick Ryan" />
					{:else}
						<div class="speaker-initials">PR</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Patrick Ryan</h3>
					<h4>VP of Observational Health Data Analytics, Janssen R&D; OHDSI Founding Collaborator</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Patrick Ryan is the Vice President of Observational Health Data Analytics at Janssen Research and Development, a division of Johnson & Johnson. He leads efforts to develop and apply advanced analytical methods to better understand the real-world effects of medical products. Dr. Ryan is also an Assistant Professor in the Department of Biomedical Informatics at Columbia University, contributing to the academic advancement of health data sciences.</p>
				<p>As a founding collaborator of Observational Health Data Sciences and Informatics (OHDSI), Dr. Ryan has been instrumental in establishing one of the world's largest and most diverse networks for observational health data analysis. His work has advanced the foundations and application of observational data analysis, methods, policies, and principles.</p>
				<p><strong>What happens when global expertise meets local data?</strong> At the symposium, Dr. Ryan will share how OHDSI's methods and networks can transform raw health data into actionable insights—empowering African researchers, guiding evidence-based decision-making, and unlocking new opportunities for innovation in patient care.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'vanzandt'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'vanzandt')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'vanzandt')?.image} alt="Mui Van Zandt" />
					{:else}
						<div class="speaker-initials">MV</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Mui Van Zandt</h3>
					<h4>VP & Global Head of Data Strategy, IQVIA; Asia-Pacific OHDSI Chapter Lead</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Mui Van Zandt is Vice-President & Global Head of Data Strategy, Access and Enablement at IQVIA and leads the Asia-Pacific chapter of OHDSI. With over 20 years of experience in software development, large-scale patient data networks and real-world evidence innovation, Mui has helped drive global adoption of the OMOP common data model and coached organizations across Asia-Pacific in data conversions, analytics and open-science collaboration.</p>
				<p>She is passionate about bridging Eastern and Western regions, mentoring emerging data scientists and fostering the spirit of collaboration and transparency in real-world data research.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'aceng'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'aceng')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'aceng')?.image} alt="Dr. Jane Ruth Aceng" />
					{:else}
						<div class="speaker-initials">JA</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Jane Ruth Aceng</h3>
					<h4>Minister of Health, Uganda</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Jane Ruth Aceng is Uganda's Minister of Health and a highly respected medical doctor, researcher, and public health leader. She has dedicated her career to strengthening Uganda's health system and improving access to quality healthcare for all.</p>
				<p>Dr. Aceng has been instrumental in guiding national health policies and responses—from HIV/AIDS and malaria programs to Uganda's COVID-19 response—earning her recognition as a trusted and visionary leader in public health. Her commitment to community health, health data use, and evidence-based decision-making continues to inspire many across Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'sung'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'sung')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'sung')?.image} alt="Prof. Cynthia Sung" />
					{:else}
						<div class="speaker-initials">CS</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Prof. Cynthia Sung, PhD</h3>
					<h4>Adjunct Associate Professor, Duke-NUS; Co-Lead, OHDSI Africa Chapter</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Prof. Cynthia Sung is a clinical pharmacologist and biomedical engineer with a distinguished career spanning government (e.g., US FDA, NIH, HSA), industry, and academia. She currently serves as Adjunct Associate Professor at Duke-NUS Medical School where she develops and teaches courses in health-products regulation. Her recent research has focused on pharmacogenomics, drug safety surveillance, real-world evidence generation, and data analytics.</p>
				<p>As Co-Lead of the OHDSI Africa Chapter since 2022, Cynthia is committed to expanding FAIR (findable, accessible, interoperable, reusable) data in under-represented populations and promoting the adoption of the OMOP Common Data Model (CDM) across Africa. She co-authored the successful EDCPT3 BRIDGE Network grant proposal that is training African PhD candidates and postdoctoral fellows in health informatics applied in the study of infectious diseases. She is also interested in transformation of data to the OMOP CDM from clinical trials and maternal and neonatal health surveys and registries. In 2023, she received the OHDSI Titan Award for Community Collaboration.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'kiragga'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'kiragga')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'kiragga')?.image} alt="Dr. Agnes Kiragga" />
					{:else}
						<div class="speaker-initials">AK</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Agnes Kiragga</h3>
					<h4>Data Science Program Lead, African Population Health Research Council</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Agnes Kiragga leads the Data Science Program at the African Population Health Research Council in Nairobi, Kenya. With over 20 years of experience, she specializes in utilizing diverse data, including African longitudinal population cohorts. She leads the Data Science Without Borders (DSWB) multi-country project, Implementation Network for Sharing Population Information from Research Entities (INSPIRE) network, and spearheads data management and analytics cores for multi-country research projects.</p>
				<p>Her work has contributed to understanding the challenges of data and AI governance, and shaped the solutions implemented in her current projects. She is keen to use data science tools applied to real-world data generated in Africa to inform global health. Dr. Kiragga strongly advocates building capacity in data systems, data harmonization, sharing, and AI governance, and applying artificial intelligence for effective decision-making in public health and livelihoods in Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'asiimwe'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'asiimwe')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'asiimwe')?.image} alt="Alex Asiimwe" />
					{:else}
						<div class="speaker-initials">AA</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Alex Asiimwe</h3>
					<h4>Head of Evidence Generation, Innovation, and Partnerships, Gilead Sciences</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Alex is a biopharmaceutical executive and health research leader with extensive experience in evidence generation, innovation, and strategic partnerships across the globe. He is also a professor of Public health. At Gilead Sciences, he drives initiatives that leverage real-world data, foster collaborations, and develop innovative health solutions to improve patient outcomes.</p>
				<p>Alex is committed to advancing access to quality healthcare, strengthening research systems, and promoting data-driven decision-making in the public and private health sectors.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'kanter'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'kanter')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'kanter')?.image} alt="Dr. Andrew S. Kanter" />
					{:else}
						<div class="speaker-initials">AK</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Andrew S. Kanter</h3>
					<h4>Assistant Professor, Columbia University; Strategic Advisor, IMO Health</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Andrew S. Kanter is a distinguished leader in health informatics and global health. He is also an Assistant Professor of Clinical Biomedical Informatics and Clinical Epidemiology at Columbia University and directs the Columbia International eHealth Lab (CIEL), focusing on eHealth initiatives in resource-limited settings. He is former Chief Medical Officer and is current strategic advisor at IMO Health (IMO), where he provides strategic direction on clinical terminology and data standardization.</p>
				<p>Dr. Kanter's work has significantly impacted health data interoperability, particularly in low-resource environments. He led the development of the Millennium Villages Global Network (MVG-Net), an open-source information system that linked clinics across Sub-Saharan Africa. He is the Terminology and Metadata lead for OpenMRS, works closely with the OHDSI, Open Concept Lab and OpenHIE communities.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'muyingo'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'muyingo')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'muyingo')?.image} alt="Dr. Sylvia Muyingo" />
					{:else}
						<div class="speaker-initials">SM</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Sylvia Muyingo</h3>
					<h4>Research Scientist, African Population and Health Research Centre (APHRC)</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Sylvia Muyingo is a research scientist at the African Population and Health Research Centre (APHRC) in Nairobi and a key figure advancing data-science innovation for population health in Africa. Her expertise spans data harmonisation, longitudinal health data systems, promoting equitable and responsible data access/use as well as translating complex datasets into actionable public-health insights.</p>
				<p>Recently, she has turned her focus to mental health—highlighting how fragmented data continues to obscure the true scale of mental-health challenges across Africa. Through initiatives like the Mental Health Data Prize Africa, she champions the power of open data, collaboration and analytics to make invisible struggles visible and measurable.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'burn'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'burn')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'burn')?.image} alt="Dr. Edward Burn" />
					{:else}
						<div class="speaker-initials">EB</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Edward Burn</h3>
					<h4>Senior Researcher, University of Oxford; DARWIN-EU & HERON-UK Projects</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Edward Burn is a senior researcher at the University of Oxford, primarily working on the DARWIN-EU and HERON-UK projects. He is a long-standing contributor to Observational Health Data Sciences and Informatics (OHDSI) network studies and open-source software. Edward's research demonstrates how open-source software and open data networks can drive global health impact.</p>
			</div>
		</div>
	</div>
{/if}



{#if expandedSpeaker === 'ario'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'ario')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'ario')?.image} alt="Prof. Dr. Alex Riolexus Ario" />
					{:else}
						<div class="speaker-initials">AA</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Prof. Dr. Alex Riolexus Ario</h3>
					<h4>Program Director, Uganda Public Health Fellowship Program; Director, Uganda National Institute of Public Health</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Prof. Dr. Alex Riolexus Ario is an Associate Professor of Infectious Disease Epidemiology, Medical Doctor and Public Health Specialist who has worked in various capacities in Uganda government and agencies. He has served as Hospital Superintendent, District Health Officer, Health Advisor, and Care and Treatment Manager in the Uganda Ministry of Health.</p>
				<p>A reviewer and editor in numerous peer-reviewed journals, he serves as Editor in Chief of the Uganda Public Health Bulletin. He has published about 300 papers in peer-reviewed journals and serves as a member in numerous Technical Working Groups, Steering Committees and Boards including the Africa CDC's Journal of Public Health in Africa, African Academy of Health Sciences, and various international health initiatives.</p>
				<p>Dr. Ario is currently the Program Director of the Uganda Public Health Fellowship Program and Director, Uganda National Institute of Public Health, Ministry of Health.</p>
				<p><strong>Can smarter health data turn insights into lifesaving action?</strong> At the symposium, Dr. Ario will explore how harmonized health information can go beyond statistics—enabling real-time, actionable decisions that save lives, strengthen health systems, and empower communities across Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'herbst'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'herbst')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'herbst')?.image} alt="Dr. Kobus Herbst" />
					{:else}
						<div class="speaker-initials">KH</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Kobus Herbst</h3>
					<h4>Director of Population Science, Africa Health Research Institute (AHRI)</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Kobus Herbst is Director of Population Science at the Africa Health Research Institute (AHRI), and a former Director of the South African Population Research Infrastructure Network (SAPRIN) at the South African Medical Research Council (SAMRC).</p>
				<p>Dr Herbst has led the development of large-scale longitudinal health and demographic surveillance systems (HDSS) in South Africa, including the integration of population data with service utilisation records, to examine causes of mortality, access to care, and health system impacts.</p>
				<p>He is a co-leader of the African Population Cohorts Consortium (APCC), a continent-wide initiative to harness population cohorts for research in Africa. His work emphasises the responsible use of large population datasets, data linkages, and research infrastructure to support evidence-based policy and health system strengthening in resource-limited settings.</p>
			</div>
		</div>
	</div>
{/if}


{#if expandedSpeaker === 'nakazibwe'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'nakazibwe')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'nakazibwe')?.image} alt="Brenda Nakazibwe" />
					{:else}
						<div class="speaker-initials">BN</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Brenda Nakazibwe</h3>
					<h4>Team Lead, Pathogen Economy, Science, Technology & Innovation Secretariat (STIS), Uganda</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Brenda Nakazibwe is currently the Team Lead for the Pathogen Economy at the Science, Technology & Innovation Secretariat (STIS) under the Office of the President of Uganda.</p>
				<p>In this role, she supports Uganda's strategy to build local capacity around diagnostics, therapeutics and vaccine development—part of a broader national push to drive science-, innovation- and industry-led health security. Her work focuses particularly on what she describes as Uganda's "pathogen economy"—leveraging research, manufacturing and regulatory systems to reduce dependency on imported health products and create value from locally-led science.</p>
				<p>Brenda has also taken part in outbreak research and public-health investigations, contributing to studies such as those looking at drivers of COVID-19 waves in Uganda.</p>
				<p><strong>What will she share at the symposium?</strong> Expect insights on how standardized health-data networks and real-world evidence platforms (like those promoted by OHDSI) can bolster national health security, anchor a sustainable pathogen economy, and accelerate translation of data into innovation.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'ganda'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'ganda')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'ganda')?.image} alt="Dr. Gregory Ganda" />
					{:else}
						<div class="speaker-initials">GG</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Gregory Ganda</h3>
					<h4>County Executive Committee Member for Health, Kisumu County, Kenya</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Gregory Ganda serves as the County Executive Committee Member (CECM) for Health in Kisumu County, Kenya. He has been instrumental in spearheading initiatives that integrate digital health solutions to enhance service delivery, particularly in underserved communities. His leadership has been pivotal in reducing maternal mortality by 30% over five years, showcasing the effectiveness of data-driven health interventions.</p>
				<p><strong>What drives Dr. Ganda's approach?</strong> His work underscores the belief that empowering community health workers with digital tools and data access is key to bridging health disparities and achieving Universal Health Coverage (UHC).</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'waynee'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'waynee')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'waynee')?.image} alt="Steven Waynee" />
					{:else}
						<div class="speaker-initials">SW</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Steven Waynee</h3>
					<h4>Founder & CEO, IntelliSOFT; President, HELINA; Secretary General, KeHIA</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Steven Waynee is a Kenyan digital health and health informatics expert with over 15 years of experience leading digital transformation in African healthcare. He is the Founder & CEO of IntelliSOFT Consulting Ltd, driving the development and implementation of EMR systems and digital health solutions in Kenya, Uganda, Mozambique, and beyond.</p>
				<p>In addition, Steven serves as President of the Pan African Health Informatics Association (HELINA) and Secretary General of the Kenya Health Informatics Association (KeHIA), advocating for digital health governance, interoperability, and capacity building. He has contributed to global initiatives like the Health Data Collaborative and the Digital Health Atlas, enabling data-driven decision-making and improving healthcare delivery across Africa.</p>
				<p><strong>How can interoperable digital health systems strengthen African healthcare?</strong> At the symposium, Steven will share insights from his experience building EMR systems, harmonizing health data, and advancing health informatics capacity on the continent.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'mbaka'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'mbaka')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'mbaka')?.image} alt="Paul Mbaka" />
					{:else}
						<div class="speaker-initials">PM</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Paul Mbaka</h3>
					<h4>Assistant Commissioner, Health Information, Ministry of Health, Uganda</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Paul Mbaka serves as Assistant Commissioner in the Department of Health Information at the Ministry of Health, Uganda. He is a statistician, monitoring & evaluation specialist, researcher and software/database developer with over a decade of experience driving data-quality, health information systems and evidence-based decision-making.</p>
				<p>Paul has been instrumental in the design, deployment and use of digital health and health-information platforms within Uganda's health system, and has championed joint innovations to improve how data is collected, integrated and used for health outcomes.</p>
				<p><strong>What insights will Paul share?</strong> Learn about leveraging health data systems, digital health innovation and the power of standardized analytics for improving care and health planning in Uganda and across Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'bouras'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'bouras')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'bouras')?.image} alt="Dr. Adam Bouras" />
					{:else}
						<div class="speaker-initials">AB</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Adam Bouras</h3>
					<h4>Founder, Tritonis Inc.; Public Health Informatics Fellow, CDC</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Adam Bouras is the founder of Tritonis Inc., a consulting and IT development company focused on health information technology. He is also a researcher in health informatics and public health; before that, he served as a Public Health Informatics Fellow at the Centers for Disease Control and Prevention (CDC). His research interests include public health informatics, epidemiology, social networks, scientific communication, and computational biology.</p>
				<p>Dr. Adam has participated in studies on health data-sharing behaviors, prioritizing the risks posed by non-communicable diseases and assessing the economic impacts of public health interventions. In addition to his academic and research efforts, Dr. Adam is actively involved with the OHDSI community, where he contributes to the standardization of health data and supports open science initiatives.</p>
				<p>Through this work, Dr. Adam is helping bridge the gap between local medication coding systems and global standards, enabling richer, more reliable real-world evidence from medication use in African and international settings. He welcomes collaborative opportunities in public health informatics and data science.</p>
				<p><strong>How can aligning just one data domain—medications—transform research collaboration, pharmacovigilance, and patient safety across Africa?</strong> Join us at the symposium to explore this important conversation with Adam Bouras.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'walravens'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'walravens')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'walravens')?.image} alt="Michel Walravens" />
					{:else}
						<div class="speaker-initials">MW</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Michel Walravens</h3>
					<h4>Rheumatologist & Researcher, Hasselt University; Developer, OikoLexis Platform</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Michel Walravens, a rheumatologist and researcher affiliated with the Biomedical Research Institute and the Data Science Institute at Hasselt University, is developing OikoLexis—a web-based platform that supports the community-driven translation of the OHDSI Book into any language.</p>
				<p>Building on his prior experience in medical terminology translation, and in response to the demand for versions of the OHDSI manual in key African languages (French, Arabic, Portuguese, Kiswahili), OikoLexis aims to expand global access to OHDSI's foundational content. Since its inception, language communities across Asia, Europe, and South America have also joined the initiative.</p>
				<p>The platform combines AI-assisted pretranslation with collaborative community review, enabling users to request translations in their preferred language and actively participate in reviewing them. OikoLexis is being developed in parallel with the creation of the source book, ensuring close alignment between the original content and its multilingual adaptations.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'atwine'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'atwine')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'atwine')?.image} alt="Mugume Atwine" />
					{:else}
						<div class="speaker-initials">MA</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Mugume Atwine</h3>
					<h4>Data Scientist, Uganda</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Mugume Twinamatsiko Atwine is a Ugandan data scientist with over five years of experience in big-data analytics, machine learning, and innovation. He has contributed to several health-data and research projects focused on applying artificial intelligence to public-health challenges. Mugume is passionate about bridging the gap between technology and real-world health solutions.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'tamirat'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'tamirat')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'tamirat')?.image} alt="Bekure Tamirat" />
					{:else}
						<div class="speaker-initials">BT</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Bekure Tamirat</h3>
					<h4>Data Science & Analytics Unit Lead, Africa CDC</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Bekure Tamirat serves as the Data Science & Analytics Unit Lead at the Africa Centres for Disease Control and Prevention (Africa CDC). He plays a pivotal role in advancing data-driven public health strategies across the continent. His leadership is instrumental in strengthening health data systems, enhancing disease surveillance, and supporting evidence-based decision-making to improve health outcomes in Africa.</p>
				<p><strong>How can harmonizing health data across Africa transform disease prevention and response strategies?</strong> Join us at the symposium to explore Bekure Tamirat's insights into building robust health data infrastructures that empower African nations to proactively address public health challenges.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'bagarukayo'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'bagarukayo')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'bagarukayo')?.image} alt="Kenneth Bagarukayo" />
					{:else}
						<div class="speaker-initials">KB</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Kenneth Bagarukayo</h3>
					<h4>Commissioner ICT Research and Development, Ministry of ICT, Uganda</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Kenneth Bagarukayo is a highly experienced and visionary ICT leader with over two decades of experience in senior-level IT management, currently serving as the Commissioner ICT Research and Development at the Ministry of ICT and National Guidance.</p>
				<p>Specializing in strategic planning, IT infrastructure, cybersecurity, and digital transformation, he directs the implementation of IT strategies, ensures robust cybersecurity, and supervises national broadband infrastructure rollouts.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'kadengye'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'kadengye')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'kadengye')?.image} alt="Dr. Damazo T. Kadengye" />
					{:else}
						<div class="speaker-initials">DK</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Damazo T. Kadengye, PhD</h3>
					<h4>Head of Data, Measurement & Evaluation, African Population and Health Research Center</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Damazo Kadengye is a multidisciplinary researcher and statistician specializing in public health, education, and epidemiology. At APHRC, he leads data measurement and evaluation initiatives, applying advanced statistical methods to large-scale population datasets. His work focuses on informing policy and program design, improving health outcomes, and strengthening research capacity across Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'tamale'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'tamale')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'tamale')?.image} alt="Dr. William Tamale" />
					{:else}
						<div class="speaker-initials">WT</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. William Tamale</h3>
					<h4>Clinical Manager, Joint Clinical Research Centre (JCRC)</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. William Tamale is a medical doctor and research leader with over 20 years of experience in clinical care and health research. At JCRC, he oversees clinical trials and research initiatives, ensuring high-quality study design and patient safety. Dr. Tamale has contributed significantly to advancing HIV, TB, and public health research in Uganda and beyond, bridging clinical practice with evidence-based medicine.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'taylor'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'taylor')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'taylor')?.image} alt="Amelia Taylor" />
					{:else}
						<div class="speaker-initials">AT</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Amelia Taylor</h3>
					<h4>Research Fellow, University of Nottingham</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Amelia Taylor is a Research Fellow in primary health care at the University of Nottingham. With a background in biomedical sciences and pharmaceutical research, she focuses on using data science, natural language processing, and visualization tools to enhance medicines safety and healthcare outcomes.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'martufi'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'martufi')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'martufi')?.image} alt="Valentina Martufi" />
					{:else}
						<div class="speaker-initials">VM</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Valentina Martufi</h3>
					<h4>Researcher, Fiocruz, Brazil</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Valentina Martufi is a researcher at the Center for Data and Knowledge Integration for Health (CIDACS) of Fiocruz Bahia, Brazil. Her work focuses on using real-world data to improve public health research and social-program evaluation in low- and middle-income countries. With experience in health systems research across Latin America and Africa, Valentina brings a global perspective on leveraging data for equitable health outcomes.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'weerasinghe'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'weerasinghe')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'weerasinghe')?.image} alt="Jayasanka Weerasinghe" />
					{:else}
						<div class="speaker-initials">JW</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Jayasanka Weerasinghe</h3>
					<h4>Product Quality Engineering Lead, OpenMRS</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Jayasanka Weerasinghe is a Software Engineer and Product Quality Engineering Lead at OpenMRS, an open-source platform improving healthcare delivery in resource-constrained settings. He focuses on strengthening digital health systems through open-source collaboration, quality engineering, and community-driven innovation.</p>
			</div>
		</div>
	</div>
{/if}


{#if expandedSpeaker === 'fankoua'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'fankoua')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'fankoua')?.image} alt="Luc Baudoin Fankoua" />
					{:else}
						<div class="speaker-initials">LF</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Luc Baudoin Fankoua</h3>
					<h4>Health Informatics & Data Science Professional, Douala General Hospital, Cameroon</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Luc Baudoin Fankoua Tchaptchet is a Health Informatics and Data Science professional at Douala General Hospital, Cameroon, where he leads digital transformation and the implementation of standardized, data-driven health systems. His work focuses on integrating clinical data with standardized frameworks to strengthen hospital operations, research, and decision-making capacity.</p>
				<p>At the symposium, Luc will share insights from the implementation of OHDSI tools and the OMOP Common Data Model (CDM) at Douala General Hospital—demonstrating how local health institutions can join global data networks to strengthen research and health outcomes across Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'saura'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'saura')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'saura')?.image} alt="Dr. Anna Saura Lázaro" />
					{:else}
						<div class="speaker-initials">AS</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Anna Saura Lázaro</h3>
					<h4>Senior Researcher, Clinical Epidemiology and RWE, University of Oxford</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Anna Saura Lázaro is a preventive medicine specialist currently working as a Senior Researcher in real-world evidence at the University of Oxford. She holds advanced degrees in public health and research methodology, and her work bridges clinical research and real-world data to strengthen evidence-based practice.</p>
				<p>With a background in HIV and infectious disease epidemiology, she brings deep expertise in population health and global health collaboration.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'belmans'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'belmans')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'belmans')?.image} alt="Dr. Luc Belmans" />
					{:else}
						<div class="speaker-initials">LB</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Luc Belmans</h3>
					<h4>CEO, Medaman</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Luc Belmans is the CEO of Medaman, a company dedicated to advancing health data interoperability and research. With expertise in medicine and health informatics, he has spent years bridging the gap between clinical care and data-driven insights.</p>
				<p>Under his leadership, Medaman supports the care record cycle, converting hospital data into the standardized OMOP Common Data Model (CDM) to enable better analysis, federated research, and evidence-based decision-making. By transforming raw clinical data into a common, harmonized format, Medaman empowers hospitals and researchers to extract actionable insights and improve patient care.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'kanyike'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'kanyike')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'kanyike')?.image} alt="Dr. Francis Kanyike" />
					{:else}
						<div class="speaker-initials">FK</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Dr. Francis Kanyike</h3>
					<h4>Public Health Specialist, Joint Clinical Research Centre (JCRC), Uganda</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Dr. Francis is a public health specialist at the Joint Clinical Research Centre (JCRC) in Uganda, with extensive experience in HIV/AIDS care, prevention, and clinical research. He has previously served as a research medical doctor with the AIDS Clinical Trials Group (ACTG) and coordinated studies for Global Health Uganda.</p>
				<p>With a strong background in program evaluation, TB/HIV coordination, and digital health implementation, Dr. Francis contributes to building data-driven health systems that support evidence-based decision-making in Uganda and across Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'descamps'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'descamps')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'descamps')?.image} alt="Freija Descamps" />
					{:else}
						<div class="speaker-initials">FD</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Freija Descamps</h3>
					<h4>Managing Partner, edenceHealth NV</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Freija Descamps leads edenceHealth NV, specialising in transforming healthcare data into actionable insights through harmonisation, analytics, and federated networks. She has a PhD in Astrophysics and extensive experience enabling real-world evidence generation and interoperable health-data infrastructures, including projects across Europe and Africa.</p>
			</div>
		</div>
	</div>
{/if}

{#if expandedSpeaker === 'nizeyimana'}
	<div class="speaker-bio-modal" on:click={() => toggleSpeaker('')} on:keypress={(e) => e.key === 'Escape' && toggleSpeaker('')} role="button" tabindex="0">
		<div class="speaker-bio-content" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="button" tabindex="0">
			<div class="speaker-bio-header">
				<div class="speaker-bio-image">
					{#if speakers.find(s => s.id === 'nizeyimana')?.image}
						<img loading="lazy" src={speakers.find(s => s.id === 'nizeyimana')?.image} alt="Pacifique Nizeyimana" />
					{:else}
						<div class="speaker-initials">PN</div>
					{/if}
				</div>
				<div class="speaker-bio-info">
					<h3>Pacifique Nizeyimana</h3>
					<h4>Rwanda Biomedical Centre (RBC)</h4>
				</div>
				<button class="close-modal-btn" on:click={() => toggleSpeaker('')}>×</button>
			</div>
			<div class="speaker-bio-body">
				<p>Pacifique Nizeyimana serves as a key leader at the Rwanda Biomedical Centre (RBC), the national health implementation agency. He represents the vital intersection between health data systems, analytics, and evidence-driven policy in Rwanda.</p>
				<p>With strong expertise in statistics, digital health, and program monitoring & evaluation, Pacifique has helped steer Rwanda's efforts to leverage data for better disease surveillance, health system performance, and informed decision-making. His work underscores how meaningful health outcomes are increasingly grounded in robust, real-time data infrastructures.</p>
			</div>
		</div>
	</div>
{/if}

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
							<p><strong>Tutorial Day (November 10):</strong></p>
							<p>Joint Clinical Research Centre (JCRC), Kampala, Uganda</p>
							
							<p style="margin-top: var(--spacing-md);"><strong>Main Conference (November 11-12):</strong></p>
							<p>Mestil Hotel, Kampala, Uganda</p>
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
							<p>Hotel rooms at Mestil Hotel may be reserved using the special booking code:</p>
							<p><strong>Booking Code:</strong> JCRC</p>
							<p>
								<strong>Booking Link:</strong> 
								<a href="https://direct-book.com/properties/MestilDIRECT?promotion_code=JCRC25" target="_blank" rel="noopener noreferrer">
									Book Now
								</a>
							</p>
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
							<p>
								Kampala is serviced by Jomo Kenyatta International Airport. The distance from the airport to the venues is approximately 28 km.
							</p>
							<p>
								Taxis are available at the airport; please use an official airport taxi. The journey takes approximately 35 minutes depending on traffic. Uber is easily accessible within the airport and for moving around Kampala.
							</p>
							<p style="margin-top: var(--spacing-md);"><strong>Travel Tip:</strong> Agree on taxi prices before departing.</p>
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
							<ul>
								<li><strong>Language:</strong> English</li>
								<li><strong>Currency:</strong> Ugandan Shilling (UGX). Major credit cards accepted. ATMs available in shopping malls.</li>
								<li><strong>Voltage:</strong> 220-240 volts with 3 square pin plugs</li>
								<li><strong>Weather in November:</strong> Average temperature 18-23°C</li>
								<li><strong>Time Zone:</strong> GMT+3</li>
							</ul>
							<p style="margin-top: var(--spacing-lg);">
								For non-OHDSI-related information, please visit the <a href="https://jcrc.org.ug" target="_blank" rel="noopener noreferrer">JCRC website</a>.
							</p>
						</div>
					{/if}
				</div>
				</div>
			</div>

			<!-- Right: Venue Image -->
			<div class="venue-image-container">
				<img loading="lazy" src="/venue-mestil-hotel.jpg" alt="Mestil Hotel - Conference Venue" class="venue-image" />
				<div class="venue-image-caption">
					<p>Mestil Hotel, Kampala</p>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- Important Dates -->
<!-- <section class="section">
	<div class="container">
		<h2 class="section-title">Important Dates</h2>
		<div class="dates-grid">
			<div class="date-card">
				<h4>Collaborator Showcase</h4>
				<ul>
					<li><strong>Submissions deadline:</strong> September 10</li>
					<li><strong>Submissions review:</strong> September 11-30</li>
					<li><strong>Notification of acceptance:</strong> October 5</li>
				</ul>
			</div>
			
			<div class="date-card">
				<h4>Symposium</h4>
				<ul>
					<li><strong>Tutorial:</strong> November 10 at JCRC</li>
					<li><strong>Main conference:</strong> November 11-12 at Mestil Hotel</li>
				</ul>
			</div>
		</div>
	</div>
</section> -->

<!-- Final CTA
<section class="section">
	<div class="container">
		<div class="cta-section">
			<h2>Join Us for This Historic Event</h2>
			<p>
				Be part of the inaugural OHDSI Africa Symposium and help shape the future of observational health research in Africa.
			</p>
			<a href="https://forms.office.com/pages/responsepage.aspx?id=4p--5enagUK1xV7iEx_EsW4qH1WABP9OiVTbq64DnSVUOEFRODJTRTQyQ0RVOFZNUEhFR09CTzRZNi4u&route=shorturl" class="btn btn-primary" target="_blank" rel="noopener noreferrer">
				Register Now for the 2025 OHDSI Africa Symposium
			</a>
		</div>
	</div>
</section> -->