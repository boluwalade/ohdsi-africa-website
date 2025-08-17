<script lang="ts">
	export let currentPage: string | null = null;
	
	const navItems = [
		{ href: '/', label: 'Home', id: '/' },
		{ href: '/about', label: 'About', id: '/about' },
		{ href: '/events', label: 'Africa Symposium 2025', id: '/events' },
		{ href: '/community', label: 'Community', id: '/community' },
		{ href: '/contact', label: 'Contact', id: '/contact' }
	];
</script>

<header class="header">
	<nav class="navbar">
		<div class="nav-container">
			<a href="/" class="nav-logo">
				<img src="/files/ohdsi-africa-logo.jpeg" alt="OHDSI Logo" class="logo">
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
			<div class="hamburger">
				<span class="bar"></span>
				<span class="bar"></span>
				<span class="bar"></span>
			</div>
		</div>
	</nav>
</header>
