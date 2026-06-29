---
layout: page
title: Greetings
permalink: /about/
---

<style type="text/css" media="screen">
	.post-header {
		display: none;
	}

	.greetings {
		--ink: #1f2937;
		--muted: #667085;
		--line: #d0d5dd;
		--paper: #ffffff;
		--soft: #f8fafc;
		--accent: #0f766e;
		--accent-soft: #ccfbf1;

		color: var(--ink);
	}

	.greetings__hero {
		display: grid;
		grid-template-columns: minmax(0, 1fr) 220px;
		gap: 36px;
		align-items: center;
		margin: 14px 0 34px;
		padding: clamp(28px, 6vw, 56px);
		border: 1px solid var(--line);
		border-radius: 18px;
		background:
			radial-gradient(circle at 92% 12%, rgba(15, 118, 110, 0.18), transparent 30%),
			linear-gradient(135deg, #ffffff 0%, #f8fafc 58%, #ecfeff 100%);
		box-shadow: 0 24px 70px rgba(15, 23, 42, 0.08);
	}

	.greetings__eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 8px;
		margin: 0 0 16px;
		color: #115e59;
		font-size: 0.82rem;
		font-weight: 700;
		letter-spacing: 0.08em;
		text-transform: uppercase;
	}

	.greetings__eyebrow::before {
		width: 10px;
		height: 10px;
		border-radius: 999px;
		background: var(--accent);
		content: "";
	}

	.greetings h1 {
		margin: 0;
		max-width: 12ch;
		font-size: clamp(2.45rem, 7vw, 5rem);
		line-height: 1;
		letter-spacing: 0;
	}

	.greetings__lead {
		margin: 22px 0 0;
		max-width: 58ch;
		color: var(--muted);
		font-size: 1.1rem;
		line-height: 1.75;
	}

	.greetings__portrait {
		position: relative;
		display: grid;
		place-items: center;
		aspect-ratio: 1;
		overflow: hidden;
		border-radius: 10px;
		background:
			linear-gradient(135deg, rgba(15, 118, 110, 0.14), rgba(255, 255, 255, 0.78)),
			repeating-linear-gradient(0deg, rgba(15, 23, 42, 0.08) 0 1px, transparent 1px 22px),
			repeating-linear-gradient(90deg, rgba(15, 23, 42, 0.08) 0 1px, transparent 1px 22px);
		box-shadow: inset 0 0 0 1px rgba(15, 118, 110, 0.2);
	}

	.greetings__portrait img {
		width: calc(100% - 18px);
		height: calc(100% - 18px);
		border-radius: 8px;
		object-fit: cover;
		box-shadow: 0 18px 38px rgba(15, 23, 42, 0.18);
	}

	.greetings__grid {
		display: grid;
		grid-template-columns: repeat(3, minmax(0, 1fr));
		gap: 16px;
		margin: 28px 0;
	}

	.greetings__card {
		padding: 22px;
		border: 1px solid var(--line);
		border-radius: 8px;
		background: var(--paper);
	}

	.greetings__card h2 {
		margin: 0 0 10px;
		font-size: 1.05rem;
	}

	.greetings__card p,
	.greetings__note p {
		margin: 0;
		color: var(--muted);
		line-height: 1.68;
	}

	.greetings__note {
		margin-top: 26px;
		padding: 24px;
		border-left: 4px solid var(--accent);
		border-radius: 8px;
		background: var(--soft);
	}

	@media (max-width: 760px) {
		.greetings__hero,
		.greetings__grid {
			grid-template-columns: 1fr;
		}

		.greetings__portrait {
			order: -1;
			max-width: 220px;
		}
	}
</style>

<div class="greetings">
	<section class="greetings__hero" aria-labelledby="greetings-title">
		<div>
			<p class="greetings__eyebrow">About this site</p>
			<h1 id="greetings-title">Hi, I am Xiao.</h1>
			<p class="greetings__lead">
				Welcome to my small corner for technical notes, practical fixes, and daily learning. I use this site to keep track of what I have tried, what worked, and what may help the next time the same problem appears.
			</p>
		</div>
		<figure class="greetings__portrait">
			<img src="{{ '/assets/about/myself.jpeg' | relative_url }}" alt="Portrait of Xiao">
		</figure>
	</section>

	<section class="greetings__grid" aria-label="What you can find here">
		<div class="greetings__card">
			<h2>Technical Notes</h2>
			<p>Short, direct write-ups about AI, Linux, DevOps, containers, automation, and the commands worth remembering.</p>
		</div>
		<div class="greetings__card">
			<h2>Real Fixes</h2>
			<p>Posts usually come from hands-on problems, so the focus is on reproducible steps and useful context.</p>
		</div>
		<div class="greetings__card">
			<h2>Daily Learning</h2>
			<p>This is also a notebook for small discoveries, experiments, and ideas that may become useful later.</p>
		</div>
	</section>

	<section class="greetings__note" aria-label="A note from Xiao">
		<p>
			Thanks for stopping by. If a note here saves you a search, explains a confusing error, or gives you a cleaner path through a technical task, then the site is doing its job.
		</p>
	</section>
</div>
