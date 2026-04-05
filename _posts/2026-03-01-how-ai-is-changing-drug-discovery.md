---
layout: post
title: "How AI is changing drug discovery - and what the evidence actually says"
description: "The industry is reorganising fast. AI-designed drugs are reaching clinical trials. Pharma companies are embedding AI infrastructure directly into their R&D pipelines. This post covers what is actually happening, what the evidence shows, and the honest open questions about whether this means better medicines."
standfirst: "The industry is reorganising fast. AI-designed drugs are reaching clinical trials. Pharma companies are embedding AI infrastructure directly into their R&D pipelines. This post covers what is actually happening, what the evidence shows, and the honest open questions about whether this means better medicines."
category: industry
category_label: "Industry & Research"
date: 2026-03-01
read_time: 10
---

Drug discovery has always been slow, expensive, and largely driven by trial and error. A new drug takes an average of ten to fifteen years to reach patients and costs somewhere between $1 billion and $2.5 billion to develop - and more than 90% of candidates that enter clinical trials still fail. The case for doing things differently has always been obvious. What has changed in the past few years is that there are now concrete tools to do it, and concrete evidence that some of them work.

This post tries to give an honest account of where things actually stand - what has been demonstrated, what is still unproven, and what the current wave of industry partnerships suggests about where this is heading.

<div class="divider"></div>

## The foundation: what the models can actually do

The most significant single development was AlphaFold. In 2021, DeepMind's AlphaFold 2 predicted protein structures with accuracy comparable to experimental methods - effectively solving a problem that the structural biology community had been working on for 50 years. By 2022, the AlphaFold Protein Structure Database had released predicted structures for over 200 million proteins, covering nearly every known organism. For drug discovery, where knowing the 3D shape of a protein target is often the starting point for designing a molecule that binds to it, this removed a significant bottleneck. AlphaFold 3, released in 2024, extended the approach to DNA, RNA, and small molecules, making it directly relevant to structure-based drug design.

ESM-2, Meta AI's protein language model trained on 250 million sequences, showed something different but equally important: that a model trained purely on sequence data - without any 3D structure information - could learn evolutionary relationships and predict functional properties. This matters because sequence data is abundant and cheap to generate in ways that structural data is not. ESM-2 and the ESMFold structure predictor it enables have become practical tools for protein engineering - I used them directly in my own research at Roche, working on antibody fitness prediction.

Newer models have expanded the toolkit further. RFdiffusion from the Baker Lab at the University of Washington uses diffusion models to design entirely new protein sequences with specified properties - binders, enzymes, and vaccine candidates designed from scratch and experimentally validated. Geneformer, from the Broad Institute, was pre-trained on 30 million single-cell transcriptomes and can predict the effects of gene perturbations in silico. These are not demonstration projects. Researchers are actively using them.

<div class="callout">
  <div class="callout-label">Worth knowing</div>
  <p>NVIDIA's BioNeMo platform wraps many of these models - ESMFold, DiffDock, MolMIM - behind accessible APIs, meaning researchers can use them without managing the underlying infrastructure. Over 100 companies were using the platform as of 2024, including Amgen, Genentech, AstraZeneca, GSK, and Novo Nordisk. This is the infrastructure layer that makes the models practically accessible at scale.</p>
</div>

## The clinical evidence: one landmark case

The most important piece of clinical evidence to date comes from Insilico Medicine. The company used its end-to-end AI platform - combining a target identification engine called PandaOmics with a generative chemistry engine called Chemistry42 - to discover a novel drug target for idiopathic pulmonary fibrosis (IPF) and design a small molecule inhibitor to hit it.

IPF is a progressive and irreversible lung disease affecting approximately 5 million people worldwide. Patients typically die within 2–5 years of diagnosis, and current treatments slow progression but do not halt it. There is a large unmet need.

The drug candidate, INS018_055, went from target identification to Phase I clinical trials in under 30 months - roughly half the traditional timeline. The Phase I results were positive. In 2023, it entered Phase II trials simultaneously in the US and China. In November 2024, Insilico reported positive Phase IIa topline results: the drug was safe and well-tolerated, and showed encouraging dose-dependent improvement in forced vital capacity - a measure of lung function - over 12 weeks. The full results were published in *Nature Biotechnology* in 2024.

<div class="source-note">Source: Insilico Medicine press release, November 2024; peer-reviewed publication in Nature Biotechnology, 2024; ClinicalTrials.gov NCT05938920.</div>

INS018_055 is the first drug discovered and designed end-to-end by generative AI to demonstrate efficacy signals in a randomised controlled trial. That is a meaningful milestone - not just for Insilico but for the broader question of whether AI-driven drug discovery actually produces clinically useful compounds. The answer, at least in this case, is a qualified yes - though Phase IIb and pivotal trials are still needed before anything definitive can be claimed about efficacy at scale.

It is worth being clear about what this does and does not show. It shows that AI tools can materially compress timelines and that AI-generated molecules can be safe and potentially effective in humans. It does not show that AI-designed drugs are systematically better than traditionally discovered ones, or that the approach will work across all disease areas. One data point is one data point.

## Two models for how pharma is organising around AI

The industry is not doing one thing. It is doing several things at once, and it is worth distinguishing between them.

### Model 1: partnering with specialist AI companies

The first model involves large pharma companies forming partnerships with specialist AI drug discovery firms, where the AI company contributes platform and capability and the pharma company contributes biological expertise, data, and clinical infrastructure.

Recursion Pharmaceuticals is probably the most prominent example. The company has built a large-scale biological operating system that translates cellular images into research findings on cellular state - and has active collaborations with Roche's Genentech and Bayer, among others. Recursion also joined NVIDIA's BioNeMo platform as the first third-party addition in 2024, making its Phenom-Beta imaging model available to other companies.

Cradle Bio focuses on protein engineering using generative AI, and has been working with pharma partners on antibody and enzyme design. Twist Bioscience sits at the synthesis end - providing the DNA and protein synthesis capabilities that make experimental validation of AI-designed molecules faster and cheaper. These companies are not replacing pharma's discovery function; they are compressing specific, expensive steps within it.

### Model 2: embedding AI infrastructure directly in-house

The second model is different in character. Rather than outsourcing AI capability to specialist firms, some large pharma companies are building it in-house by partnering directly with NVIDIA to install serious compute infrastructure and access BioNeMo's model ecosystem.

In late 2023, Roche's Genentech signed a multiyear collaboration with NVIDIA, with the explicit goal of enhancing its existing AI research using BioNeMo and running its "lab in a loop" framework - a continuous cycle where experimental data feeds back into AI model training and improves molecular designs.

In early 2024, Amgen announced it would deploy an NVIDIA DGX SuperPOD - a full-stack AI supercomputer with 248 H100 Tensor Core GPUs - at its deCODE Genetics subsidiary in Reykjavik, Iceland. The system, named Freyja, is specifically designed to train AI models on deCODE's genomic dataset, one of the largest and most comprehensive human genetic resources in the world. The goal is to build precision medicine models for discovering drug targets and disease biomarkers.

The most significant announcement in this category came in January 2026. NVIDIA and Eli Lilly announced a joint investment of up to $1 billion over five years to build a co-innovation lab in San Francisco, where Lilly's biologists and NVIDIA's AI engineers will work side by side. The lab will be built on BioNeMo and NVIDIA's Vera Rubin architecture, and will focus on molecule design and simulation, clinical development optimisation, and manufacturing. It has been described as the largest disclosed AI collaboration in the pharmaceutical industry to date.

<div class="source-note">Sources: BioPharma Dive, November 2023 (Genentech-NVIDIA); Drug Discovery Trends, January 2024 (Amgen-NVIDIA); NVIDIA Newsroom, January 2026 (Lilly-NVIDIA).</div>

The scale of the Lilly deal is notable. It is not a research collaboration or a platform access agreement. It is a physical infrastructure investment with engineers co-located - closer in character to a joint venture than a standard pharma-tech partnership. That suggests Lilly, at least, has concluded that AI capability needs to be embedded in the core of drug discovery rather than bolted on.

## The honest open questions

None of this means the hard problems are solved. A few things are worth being clear-eyed about.

The Insilico result is encouraging but preliminary. Phase IIa is designed to assess safety and get early efficacy signals, not to prove that a drug works at scale. INS018_055 still needs to succeed in a larger pivotal trial before it can be prescribed to patients. The history of drugs that looked promising in Phase II and failed in Phase III is long.

AI models are very good at optimising for properties they have been trained on. They are less reliable when it comes to properties that are harder to measure - off-target effects, long-term toxicity, behaviour in diverse patient populations. The molecular spaces these models explore are vast, but our ability to evaluate what we find there is still limited by how much wet-lab experimental data exists to train on.

There is also a question about whether the benefits will be evenly distributed. The companies best positioned to use these tools are the ones with the largest proprietary datasets, the most compute, and the most ML expertise. That is a small number of very large organisations and very well-funded startups. Whether this leads to better medicines for neglected diseases and underserved populations, or primarily accelerates discovery in commercially attractive therapeutic areas, depends on choices the industry has not yet made.

## What this means in practice

For working scientists, the practical implication is not that AI will replace drug discovery but that it is changing which parts of it are the bottlenecks. Target identification, structural prediction, lead generation, and molecular optimisation are all areas where AI tools are now genuinely useful - not as replacements for biological intuition but as ways to explore larger spaces faster and to filter candidates more efficiently before committing to expensive experimental work.

The challenge is that accessing and applying these tools still requires an unusual combination of skills. Most bench scientists do not have the computational background to run BioNeMo or fine-tune a protein language model. Most ML engineers do not have the biological training to know what questions to ask or how to interpret the output. Closing that gap - which is partly a tooling problem, partly a training problem, and partly a communication problem - is what I think about most, and what drives most of what I write and build.
