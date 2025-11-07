<script lang="ts">
	import { browser } from '$app/environment';
	
	export let currentPage: string | null = null;
	
	let mobileMenuOpen = false;
	
	const navItems = [
		{ href: '/', label: 'Home', id: '/' },
		{ href: '/about', label: 'About', id: '/about' },
		{ href: '/events', label: 'Africa Symposium 2025', id: '/events' },
		{ href: '/community', label: 'Community', id: '/community' },
		{ href: '/contact', label: 'Contact', id: '/contact' }
	];

	function toggleMobileMenu() {
		mobileMenuOpen = !mobileMenuOpen;
		// Prevent body scroll when menu is open (browser only)
		if (browser) {
			if (mobileMenuOpen) {
				document.body.style.overflow = 'hidden';
			} else {
				document.body.style.overflow = '';
			}
		}
	}

	function closeMobileMenu() {
		mobileMenuOpen = false;
		if (browser) {
			document.body.style.overflow = '';
		}
	}

	// Handle escape key
	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Escape' && mobileMenuOpen) {
			closeMobileMenu();
		}
	}
</script>

<svelte:window on:keydown={handleKeydown} />

<header class="header">
	<nav class="navbar">
		<div class="nav-container">
			<a href="/" class="nav-logo">
				<img loading="lazy" src="/files/ohdsi-africa-logo.jpeg" alt="OHDSI Logo" class="logo">
			</a>
			<div class="nav-right">
				<ul class="nav-menu">
					{#each navItems as item}
						<li class="nav-item">
							<a 
								href={item.href} 
								class="nav-link" 
								class:active={currentPage === item.id}
							>
								{item.label}
							</a>
						</li>
					{/each}
				</ul>
			</div>
			<button 
				class="hamburger" 
				class:active={mobileMenuOpen}
				on:click={toggleMobileMenu}
				aria-label="Toggle mobile menu"
				aria-expanded={mobileMenuOpen}
			>
				<span class="bar"></span>
				<span class="bar"></span>
				<span class="bar"></span>
			</button>
		</div>

		<!-- Mobile Menu Overlay -->
		{#if mobileMenuOpen}
			<div class="mobile-menu-overlay" on:click={closeMobileMenu} on:keypress={(e) => e.key === 'Enter' && closeMobileMenu()} role="button" tabindex="0">
				<div class="mobile-menu" on:click={(e) => e.stopPropagation()} on:keypress={(e) => e.stopPropagation()} role="menu" tabindex="0">
					<ul class="mobile-nav-menu">
						{#each navItems as item}
							<li class="mobile-nav-item">
								<a 
									href={item.href} 
									class="mobile-nav-link" 
									class:active={currentPage === item.id}
									on:click={closeMobileMenu}
								>
									{item.label}
								</a>
							</li>
						{/each}
					</ul>
				</div>
			</div>
		{/if}
	</nav>
</header>
