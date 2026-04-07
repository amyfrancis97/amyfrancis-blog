---
layout: post
title: "How AI is changing drug discovery - and what the evidence points towards"
description: "The industry is reorganising. AI-designed drugs are reaching clinical trials. Pharma companies are embedding AI infrastructure directly into their R&D pipelines. This post covers the most recent milestones, what the evidence suggests, and the honest open questions about whether this could lead to better medicines."
standfirst: "The industry is reorganising. AI-designed drugs are reaching clinical trials. Pharma companies are embedding AI infrastructure directly into their R&D pipelines. This post covers the most recent milestones, what the evidence suggests, and the honest open questions about whether this could lead to better medicines."
category: industry
category_label: "Industry & Research"
date: 2026-03-01
read_time: 13
---

Drug discovery has always been slow, expensive, and largely driven by trial and error. A new drug takes an average of ten to fifteen years to reach patients and costs somewhere between $1 billion and $2.5 billion to develop - and more than 90% of candidates that enter clinical trials still fail. The case for doing things differently has always been obvious. What has changed in the past few years is that there are now concrete tools to do it, and concrete evidence that some of them work.

This post tries to give an honest account of where things actually stand - what has been demonstrated, what is still unproven, and what the current wave of industry partnerships suggests about where this is heading.

<div class="divider"></div>

## The foundation: what the models can actually do

The most significant single development was AlphaFold. In 2021, DeepMind's AlphaFold 2 predicted protein structures with accuracy comparable to experimental methods - effectively solving a problem that the structural biology community had been working on for 50 years. By 2022, the AlphaFold Protein Structure Database had released predicted structures for over 200 million proteins, covering nearly every known organism. For drug discovery, where knowing the 3D shape of a protein target is often the starting point for designing a molecule that binds to it, this removed a significant bottleneck. AlphaFold 3, released in 2024, extended the model to DNA, RNA, and small molecules, making it directly relevant to structure-based drug design.

ESM-2, Meta AI's protein language model trained on 250 million sequences, showed something different but equally important: that a model trained purely on sequence data - without any 3D structure information - could learn evolutionary relationships and predict functional properties. This matters because sequence data is abundant and cheap to generate in ways that structural data is not. ESM-2 and the ESMFold structure predictor it enables have become practical tools for protein engineering - ones I used directly in my own research at Roche, working on antibody fitness prediction.

Newer models have expanded the toolkit further. RFdiffusion from the Baker Lab at the University of Washington uses diffusion models to design entirely new protein sequences with specified properties - binders, enzymes, and vaccine candidates designed from scratch and experimentally validated. Geneformer, from the Broad Institute, was pre-trained on 30 million single-cell transcriptomes and can predict the effects of gene perturbations in silico, acting as a hypothesis-generation tool for target identification rather than a definitive predictor.

<div class="callout">
  <div class="callout-label">Worth knowing</div>
  <p>NVIDIA's BioNeMo platform wraps many of these models - ESMFold, DiffDock, MolMIM - behind accessible APIs, meaning researchers can use them without managing the underlying infrastructure. Over 100 companies were using the platform as of 2024, including Amgen, Genentech, AstraZeneca, GSK, and Novo Nordisk. This is the infrastructure layer that makes the models practically accessible at scale.</p>
</div>

## The clinical evidence: one landmark case

The most important piece of clinical evidence to date comes from Insilico Medicine. The company used its end-to-end AI platform - combining a target identification engine called PandaOmics with a generative chemistry engine called Chemistry42 - to discover a novel drug target for idiopathic pulmonary fibrosis (IPF) and design a small molecule inhibitor to target it. IPF is a progressive and irreversible lung disease affecting approximately 5 million people worldwide; patients typically die within 2–5 years of diagnosis, and current treatments slow progression but do not halt it.

The drug candidate, INS018_055, went from target identification to Phase I clinical trials in under 30 months - roughly half the traditional timeline. The Phase I results were positive, and in 2023 it entered Phase II trials simultaneously in the US and China. In November 2024, Insilico reported positive Phase IIa topline results: the drug was safe and well-tolerated, and showed encouraging dose-dependent improvement in forced vital capacity - a measure of lung function - over 12 weeks. The full results were published in *Nature Biotechnology* in 2024.

<div class="source-note">Source: Insilico Medicine press release, November 2024; peer-reviewed publication in Nature Biotechnology, 2024; ClinicalTrials.gov NCT05938920.</div>

INS018_055 is the first drug discovered and designed end-to-end by generative AI to demonstrate efficacy signals in a randomised controlled trial - a significant milestone not just for Insilico but for the broader question of whether AI-driven drug discovery actually produces clinically useful compounds. The answer, at least in this case, is a qualified yes, though Phase IIb and pivotal trials are still needed before anything definitive can be claimed about efficacy at scale. It shows that AI tools can materially compress timelines and that AI-generated molecules can be safe and potentially effective in humans - but it does not show that AI-designed drugs are systematically better than traditionally discovered ones, or that the approach will generalise across all disease areas.

## Two models for how pharma is organising around AI

Rather than converging on a single approach, the industry is organising around AI in two quite different ways, and it is worth distinguishing between them.

### Model 1: partnering with specialist AI companies

The first model involves large pharma companies forming partnerships with AI-native firms, where the AI company contributes platform and capability and the pharma company contributes biological expertise, proprietary data, and clinical infrastructure. Recursion Pharmaceuticals is probably the most prominent example, with a large-scale biological operating system that translates cellular images into research findings on cellular state and active collaborations with Genentech and Bayer. Cradle Bio focuses on protein engineering using generative AI, while Twist Bioscience sits at the synthesis end - providing the DNA and protein synthesis capabilities that make experimental validation of AI-designed molecules faster and cheaper. These companies are not replacing pharma's discovery function; they are compressing specific, expensive steps within it, in a model that is relatively modular and low-risk for large organisations that want to adopt AI capabilities without fully rebuilding their internal systems.

### Model 2: embedding AI infrastructure directly in-house

The second model is more structural. Rather than outsourcing capability to specialist firms, some companies are embedding AI directly into their R&D stack through compute infrastructure, internal platforms, and long-term partnerships built around a "lab-in-the-loop" paradigm, where experimental data feeds continuously back into model training and models guide the next round of experiments.

In late 2023, Roche's Genentech signed a multiyear collaboration with NVIDIA to enhance its AI research using BioNeMo and run this kind of continuous loop. In early 2024, Amgen deployed an NVIDIA DGX SuperPOD - 248 H100 GPUs - at its deCODE Genetics subsidiary in Iceland, specifically to train models on one of the largest human genomic datasets in the world. The most significant announcement in this space came in January 2026, when NVIDIA and Eli Lilly announced a joint investment of up to $1 billion over five years to build a co-innovation lab in San Francisco, with Lilly's biologists and NVIDIA's AI engineers working side by side - the largest disclosed AI collaboration in the pharmaceutical industry to date, and closer in character to a joint venture than a standard pharma-tech partnership.

<div class="source-note">Sources: BioPharma Dive, November 2023 (Genentech-NVIDIA); Drug Discovery Trends, January 2024 (Amgen-NVIDIA); NVIDIA Newsroom, January 2026 (Lilly-NVIDIA).</div>

The scale of the Lilly deal suggests that at least some large companies have concluded that AI capability needs to be embedded in the core of drug discovery rather than bolted on - which is a different kind of commitment than signing a platform access agreement.

## Where the bottlenecks are now

The most important shift is not that drug discovery has become easy - it is that the bottlenecks have moved. AI systems are now quite good at predicting structure, generating candidate molecules, and exploring large design spaces. They are much less reliable when it comes to toxicity, off-target effects, pharmacokinetics, and long-term safety in humans - properties often grouped under ADMET (absorption, distribution, metabolism, excretion, toxicity) - and these are where most drugs fail. Progress here has been slower partly because the underlying data is sparse, noisy, and difficult to generate at scale, which points to a deeper constraint: model performance is increasingly limited not by architecture but by the availability of high-quality, well-annotated experimental and clinical data, especially negative results, which are rarely shared.

This is one area where the lab-in-the-loop model could turn out to matter more than it might initially seem. A compound that fails an assay is still informative data - it tells you something about the relationship between structure and toxicity, or binding and selectivity, that a successful compound does not. Drug discovery has always generated this information; it has just had no systematic way of learning from it, because failed experiments rarely get published and the data rarely leaves the organisation that produced it. Closed-loop systems do not solve the broader problem of sharing negative results across the industry, but they do at least mean that individual companies could stop quietly discarding their own losses - and start treating failure as training data.

There is also a persistent gap between retrospective and prospective performance. Many models perform well on held-out benchmark datasets but have not been consistently validated in real-world discovery settings, and closing that gap is one of the central challenges for the field. Target identification remains fundamentally hard too - we are getting better at designing molecules once a target is known, but selecting the right targets in the first place, those that are both biologically causal and therapeutically tractable, remains a major source of failure that AI has not yet meaningfully addressed.

## The honest open questions

None of this means the hard problems are solved. There is still no clear evidence that AI-discovered drugs have higher clinical success rates than traditionally discovered ones - that may change as more candidates progress through trials, but it has not yet been demonstrated. Models can optimise well for properties they are trained on, but many clinically important outcomes are difficult to measure or predict in advance, and the molecular spaces these systems explore are vast in ways that our experimental capacity to validate is not.

Regulation is an evolving factor that is easy to underestimate. Agencies are still working out how to evaluate AI-assisted discovery pipelines, particularly in terms of reproducibility, validation, and transparency, and this will shape how quickly these approaches can be adopted at scale and what evidence standards companies will need to meet.

There are also structural questions about who benefits. The organisations best positioned to use these tools are those with large proprietary datasets, significant compute, and strong ML expertise - a relatively small group of very large companies and well-funded startups. Whether this ultimately leads to better medicines for neglected diseases and underserved populations, or primarily accelerates discovery in commercially attractive areas, depends on decisions the industry has not yet made.

## What this means

For working scientists, the practical implication is not that AI replaces drug discovery, but that it changes where effort is spent. Target identification, structural prediction, lead generation, and molecular optimisation are all areas where AI tools can now materially accelerate progress, and the limiting factor is increasingly how well those tools are integrated with experimental workflows - and how well scientists can interpret and act on what the models produce.

The challenge is that using them effectively still requires an unusual combination of skills. Most bench scientists do not have the computational background to work directly with these systems, and most ML engineers do not have the biological context needed to ask the right questions or interpret results. Closing that gap - through better tools, better training, and better communication across disciplines - is likely to matter as much as the models themselves.

Because at this point, the question is no longer whether AI can contribute to drug discovery. It is whether we can build the systems, organisations, and workflows needed to make that contribution reliable.
