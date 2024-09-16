---
title: "NerveTracker: a Python-based software toolkit for visualizing and tracking groups of nerve fibers in serial block-face microscopy with ultraviolet surface excitation images"
collection: publications
category: manuscripts
permalink: /publication/2024-06-18-tractography
excerpt: 'Created a software for microscopic tractography in peripheral nerves. Inspired by similar toolkits that process diffusion MR datasets of the brain.'
date: 2024-06-18
venue: 'Jounal of Biomedical Optics'
slidesurl: 
paperurl: 'https://www.ncbi.nlm.nih.gov/pmc/articles/PMC11188586/'
citation: 
---

Information about the spatial organization of fibers within a nerve is crucial to our understanding of nerve anatomy and its response to neuromodulation therapies. A serial block-face microscopy method [three-dimensional microscopy with ultraviolet surface excitation (3D-MUSE)] has been developed to image nerves over extended depths ex vivo. To routinely visualize and track nerve fibers in these datasets, a dedicated and customizable software tool is required.  

Our objective was to develop custom software that includes image processing and visualization methods to perform microscopic tractography along the length of a peripheral nerve sample.  

We modified common computer vision algorithms (optic flow and structure tensor) to track groups of peripheral nerve fibers along the length of the nerve. Interactive streamline visualization and manual editing tools are provided. Optionally, deep learning segmentation of fascicles (fiber bundles) can be applied to constrain the tracts from inadvertently crossing into the epineurium. As an example, we performed tractography on vagus and tibial nerve datasets and assessed accuracy by comparing the resulting nerve tracts with segmentations of fascicles as they split and merge with each other in the nerve sample stack.  

We found that a normalized Dice overlap (Dicenorm) metric had a mean value above 0.75 across several millimeters along the nerve. We also found that the tractograms were robust to changes in certain image properties (e.g., downsampling in-plane and out-of-plane), which resulted in only a 2% to 9% change to the mean Dicenorm values. In a vagus nerve sample, tractography allowed us to readily identify that subsets of fibers from four distinct fascicles merge into a single fascicle as we move ∼5  mm along the nerve’s length.  

Overall, we demonstrated the feasibility of performing automated microscopic tractography on 3D-MUSE datasets of peripheral nerves. The software should be applicable to other imaging approaches. The code is available at [this link](https://github.com/ckolluru/NerveTracker.)