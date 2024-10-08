---
title: "Imaging peripheral nerves with serial block-face microscopy"
excerpt: "PhD dissertation<br/><img src='/images/portfolio/phd_research_crop2.png' height='300'>"
collection: portfolio
---

There is a need to learn about how nerves are arranged in the human peripheral nervous system. Peripheral nerves often innervate multiple organs and understanding their connectivity maps is key to being able to deliver targeted neuromodulation therapies. The vagus nerve is one such example. It provides parasympathetic innervation to the heart, lungs, stomach, and several other internal organs. By understanding where neurons that innervate the heart are located within the cross-section where electrodes are placed, researchers would be able to design and develop targeted therapies, for instance, to treat chronic heart failure.

![Motivation](/images/portfolio/phd_research_1.png)

This is a challenging project since these nerves are thin and long structures, ranging up to 80 centimeres in length and several millimeters in diameter. The resolution versus field of view trade-off with medical imaging also makes this an interesting problem to solve. With traditional histology and light microscopy, thin sections need to be cut from the sample, stained and examined on glass slides, making the process quite cumbersome. [Volume EM methods](https://doi.org/10.1038/s41592-023-01861-8) are slowly growing, but samples are still need to be well under a few millimeters in all three dimensions. [Light microscopy with optical clearing](https://doi.org/10.5858%2Farpa.2018-0466-OA) has been applied to image long, thin samples (such as prostate biopsy needle cores), but the method is yet to be validated on cadaver nerve tissues. 

The group at CWRU first developed methods using [micro-CT](https://doi.org/10.1088/1741-2552/ac9643) for this problem, and showed that fascicle bundles (packed groups of individual nerve fibers/neurons) have a complex plexiform structure, even when analyzed across 5 centimeters of length. For such an organization, it becomes necessary to view the nerve at a higher resolution, to see how individual nerve fibers move across fascicles. 

![Histology and fascicle organization](/images/portfolio/phd_research_2.png)

I built sample preparation, imaging and image analysis methods for a prototype system for this task, using concepts from other serial block-face imaging systems and nerve staining methods from EM. I used [MUSE microscopy](https://doi.org/10.1038/s41551-017-0165-y) in a serial block-face imaging setup. The method was able to analyze blocks up to 12 mm in length, 2-3 mm in diameter and with 3-4 micron resolution in plane. The sectioning thickness can be set to 3 microns as well, since the blocks are embedded in a plastic resin, namely, glycol methacrylate. We were able to identify examples of fascicles merging and splitting, and fibers moving between them. With methods from computer vision (optic flow and structure tensor analysis), we were able to track these movements and create representative tractograms. These representations are similar to those done in the [brain connectome project](https://www.humanconnectome.org/), but for the peripheral nervous system.

![Methods](/images/portfolio/phd_research_crop2.png)

Image slices at regular spacing within an image stack are shown below. The dark rings are the myelin sheaths of a myelinated neuron. As we progress in the stack, we identify a small group of fibers (dashed lines) moving from one fascicle to another.

![Results - slices](/images/portfolio/muse_results.png)

