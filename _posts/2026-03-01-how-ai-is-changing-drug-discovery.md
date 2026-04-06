---
layout: post
title: "How AI is changing drug discovery - and what the evidence actually says"
description: "The industry is reorganising fast. AI-designed drugs are reaching clinical trials. Pharma companies are embedding AI infrastructure directly into their R&D pipelines. This post covers what is actually happening, what the evidence shows, and the honest open questions about whether this means better medicines."
standfirst: "The industry is reorganising fast. AI-designed drugs are reaching clinical trials. Pharma companies are embedding AI infrastructure directly into their R&D pipelines. This post covers what is actually happening, what the evidence shows, and the honest open questions about whether this means better medicines."
category: industry
category_label: "Industry & Research"
date: 2026-03-01
read_time: 13
---

Drug discovery has always been slow, expensive, and heavily shaped by trial and error. Bringing a new drug to market typically takes 10–15 years and is often estimated to cost between $1 billion and $2.5 billion, depending on how those costs are calculated. More than 90% of candidates that enter clinical trials still fail. The case for doing things differently has always been obvious.

What has changed over the past few years is that there are now concrete tools to do it - and early evidence that some of them work.

This post tries to give a clear account of where things actually stand: what has been demonstrated, what remains unproven, and what current industry behaviour suggests about where this is heading.

<div class="divider"></div>

## The foundation: what the models can actually do

The most significant single development was AlphaFold. In 2021, DeepMind's AlphaFold 2 predicted protein structures with accuracy comparable to experimental methods - effectively solving large parts of a problem that the structural biology community had worked on for decades. By 2022, the AlphaFold Protein Structure Database had released predicted structures for over 200 million proteins, covering nearly every known organism. Later iterations extended the approach to protein complexes and interactions with DNA, RNA, and small molecules, making them more directly useful for structure-based drug design.

That said, these systems are not universal replacements for experimental methods. Dynamic conformations, disordered regions, and context-dependent interactions still require lab validation - and the structures themselves are predictions, not measurements.

Protein language models showed something different but equally important. ESM-2, Meta AI's model trained on 250 million sequences, demonstrated that a model trained purely on sequence data - without any 3D structural input - could learn meaningful representations of protein biology. Their importance lies less in raw accuracy and more in the fact that sequence data is abundant, cheap, and continually expanding. In practice these models are most powerful when adapted to specific tasks - such as predicting mutation effects or guiding protein engineering - rather than as general-purpose predictors. I used ESM-2 and ESMFold directly in my own research at Roche, working on antibody fitness prediction.

Newer generative approaches have expanded the toolkit further. RFdiffusion from the Baker Lab at the University of Washington uses diffusion models to generate entirely new protein sequences with specified properties - binders, enzymes, and vaccine candidates designed from scratch and experimentally validated. These have been validated in multiple proof-of-concept studies, but are still early in terms of robustness and repeatability across targets.

At the cellular level, large-scale models trained on transcriptomic data - such as Geneformer from the Broad Institute, pre-trained on 30 million single-cell transcriptomes - can now predict the effects of gene perturbations in silico. These are beginning to act as hypothesis-generation tools for target identification, rather than definitive predictors.

Taken together, these systems change the shape of early discovery. They make it possible to explore much larger regions of chemical and biological space, and to do so far more quickly than before. But they do not remove the need for experimental validation - and they shift, rather than eliminate, the key bottlenecks.

<div class="callout">
  <div class="callout-label">Worth knowing</div>
  <p>NVIDIA's BioNeMo platform wraps many of these models - ESMFold, DiffDock, MolMIM - behind accessible APIs, meaning researchers can use them without managing the underlying infrastructure. Over 100 companies were using the platform as of 2024, including Amgen, Genentech, AstraZeneca, GSK, and Novo Nordisk. This is the infrastructure layer that makes the models practically accessible at scale.</p>
</div>

## The clinical evidence: one landmark case

The most important clinical data point to date comes from Insilico Medicine. The company used its end-to-end AI platform - combining a target identification engine called PandaOmics with a generative chemistry engine called Chemistry42 - to discover a novel drug target for idiopathic pulmonary fibrosis (IPF) and design a small molecule inhibitor to hit it.

IPF is a progressive and irreversible lung disease affecting approximately 5 million people worldwide. Patients typically die within 2–5 years of diagnosis, and existing therapies slow progression but do not halt it.

The candidate, INS018_055, moved from target identification to Phase I trials in under 30 months - roughly half the traditional timeline. Phase I results showed the drug was safe and well-tolerated. In 2023, it entered Phase II trials simultaneously in the US and China. In November 2024, Insilico reported positive Phase IIa topline results: the drug showed encouraging dose-dependent improvement in forced vital capacity - a measure of lung function - over 12 weeks. The full results were published in *Nature Biotechnology* in 2024.

<div class="source-note">Source: Insilico Medicine press release, November 2024; peer-reviewed publication in Nature Biotechnology, 2024; ClinicalTrials.gov NCT05938920.</div>

This is a meaningful milestone. It shows that AI-assisted approaches can compress timelines and produce molecules that are viable in humans. But it is important to be precise about what this does and does not demonstrate.

Phase IIa trials are designed to assess safety and generate early efficacy signals - not to prove clinical benefit at scale. Many drugs that look promising at this stage fail in larger trials. INS018_055 still needs to succeed in pivotal studies before any definitive claims can be made. More broadly, this is one data point. It demonstrates possibility, not generality.

## Two models for how pharma is organising around AI

The industry is not converging on a single approach. Two distinct models are emerging.

### Model 1: partnering with specialist AI companies

In this model, large pharma companies partner with AI-native firms that provide specific capabilities - molecular design, protein engineering, phenotypic screening - while the pharma company contributes biological expertise, proprietary data, and clinical development capacity.

Recursion Pharmaceuticals is probably the most prominent example, with active collaborations with Genentech and Bayer. Cradle Bio focuses on protein engineering using generative AI. Twist Bioscience provides the DNA and protein synthesis capabilities that make experimental validation of AI-designed molecules faster and cheaper.

These companies are not replacing pharma's discovery function. They are compressing particular steps within it. The model is modular and relatively low-risk - it allows large organisations to adopt AI capabilities without fully rebuilding their internal systems.

### Model 2: embedding AI infrastructure directly in-house

The second model is more structural. Rather than outsourcing capability, some companies are embedding AI directly into their R&D stack through compute infrastructure, internal platforms, and long-term partnerships built around a "lab-in-the-loop" paradigm: experimental data feeds continuously back into model training, and models guide the next round of experiments.

In late 2023, Roche's Genentech signed a multiyear collaboration with NVIDIA to enhance its AI research using BioNeMo and run exactly this kind of continuous loop. In early 2024, Amgen deployed an NVIDIA DGX SuperPOD - 248 H100 GPUs - at its deCODE Genetics subsidiary in Iceland, specifically to train models on one of the largest human genomic datasets in the world.

The most significant announcement came in January 2026: NVIDIA and Eli Lilly announced a joint investment of up to $1 billion over five years to build a co-innovation lab in San Francisco, with Lilly's biologists and NVIDIA's AI engineers working side by side. It is the largest disclosed AI collaboration in the pharmaceutical industry to date - closer in character to a joint venture than a standard partnership.

<div class="source-note">Sources: BioPharma Dive, November 2023 (Genentech-NVIDIA); Drug Discovery Trends, January 2024 (Amgen-NVIDIA); NVIDIA Newsroom, January 2026 (Lilly-NVIDIA).</div>

This approach is more capital-intensive and organisationally demanding, but potentially more powerful. It treats AI not as a tool bolted onto discovery, but as core infrastructure for it.

## Where the real bottlenecks are now

The most important shift is not that drug discovery has become easy - it is that the bottlenecks have moved.

AI systems are now quite good at predicting structure, generating candidate molecules, and exploring large design spaces. They are much less reliable when it comes to toxicity, off-target effects, pharmacokinetics, and long-term safety in humans. These properties are often grouped under ADMET (absorption, distribution, metabolism, excretion, toxicity) - and they are where most drugs fail. Progress here has been slower, partly because the underlying data is sparse, noisy, and difficult to generate at scale.

This points to a deeper constraint: data quality. Model performance is increasingly limited not by architecture, but by the availability of high-quality, well-annotated experimental and clinical data - especially negative results, which are rarely shared.

There is also a gap between retrospective and prospective performance. Many models perform well on held-out benchmark datasets but have not been consistently validated in real-world discovery settings. Closing that gap is one of the central challenges for the field.

And target identification remains fundamentally hard. We are getting better at designing molecules once a target is known - but selecting the right targets in the first place, those that are both biologically causal and therapeutically tractable, remains a major source of failure that AI has not yet meaningfully addressed.

## The honest open questions

None of this means the hard problems are solved.

There is still no clear evidence that AI-discovered drugs have higher clinical success rates than traditionally discovered ones. That may change as more candidates progress through trials, but it has not yet been demonstrated.

Evaluation remains a core challenge. Models can optimise for properties they are trained on, but many clinically important outcomes are difficult to measure or predict in advance. The molecular spaces these models explore are vast - our ability to evaluate what we find there is still limited by how much wet-lab data exists to train on.

Regulation is an evolving factor that is easy to underestimate. Agencies are still working out how to evaluate AI-assisted discovery pipelines, particularly in terms of reproducibility, validation, and transparency. This will shape how quickly these approaches can be adopted at scale and what evidence standards companies will need to meet.

There are also structural questions about who benefits. The organisations best positioned to use these tools are those with large proprietary datasets, significant compute resources, and strong ML expertise. That is a small number of very large organisations and well-funded startups. Whether this leads to better medicines for neglected diseases and underserved populations - or primarily accelerates discovery in commercially attractive areas - depends on decisions the industry has not yet made.

## What this means in practice

For working scientists, the practical implication is not that AI replaces drug discovery. It is that it changes where effort is spent.

Target identification, structural prediction, lead generation, and molecular optimisation are all areas where AI tools can now materially accelerate progress. The limiting factor is increasingly how well these tools are integrated with experimental workflows - and how well scientists can interpret and act on what the models produce.

The challenge is that using them effectively still requires an unusual combination of skills. Most bench scientists do not have the computational background to work directly with these systems. Most ML engineers do not have the biological context needed to ask the right questions or interpret results. Closing that gap - through better tools, better training, and better communication - is likely to matter as much as the models themselves.

Because at this point, the question is no longer whether AI can contribute to drug discovery.

It is whether we can build the systems, organisations, and workflows needed to make that contribution reliable.
