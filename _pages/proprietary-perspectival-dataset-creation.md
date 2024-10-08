---
permalink: /proprietary-perspectival-dataset-creation
title: Proprietary & Perspectival Dataset Creation
toc: true
toc_sticky: true
author_profile: false
---

In the past few weeks, we’ve explored the complexities of representing cultural phenomena as data, delving into its historical roots, political dimensions, and how frameworks like data feminism and data archaeology reveal the biases and exclusions embedded within datasets. But the best way to truly grasp these issues is by getting hands-on and creating our own datasets.

In this assignment, you will examine how proprietary platforms can restrict or mediate our understanding of cultural data, while also exploring how to leverage perspectival or subjective analysis to construct new datasets that push back against these limitations. The goal is to envision you are creating datasets for researchers in the future who may not have access to the same platforms or data that you do. So consider what would you want them to know about experiences on these platforms and how you can best represent that in a dataset.

## Part 1: Selecting & Accessing Platform Data

As we have learned from our readings so far, there are a number of challenges when accessing data from proprietary platforms. These platforms, such as Facebook, Twitter, or Instagram, have increasingly restricted access to their data since the rise of AI tools since that data is useful for training models. However, these restrictions can make it difficult for researchers to access and analyze data from these platforms.

**Step 1. Select a Platform**

As a group, choose a proprietary platform that you would like to explore. This could be a social media platform, a streaming service, a search engine, or any other platform that collects and stores data. You can select one that either is relevant to your group project OR where the majority of your group members have experience/accounts.

**Step 2. Review Terms of Service & Access Limitations**

Detailed below are the steps you should take to explore the platform's terms of service regarding data access:

- [ ] Locate and investigate the platform’s terms of service. Does it mention anything regarding data access? How clear is it on whether users can download their personal data?
- [ ] Research if the platform offers any bulk or academic access options and whether this requires a fee or application to utilize it.
- [ ] Identify how, if at all, researchers are accessing data from this platform. Are there any tools or APIs that facilitate this process?
- [ ] Using resources like the Internet Archive’s Wayback Machine, explore how these terms of service have changed since the platform first launched. Have restrictions increased or decreased over time?

**Step 3. Document Findings**

In your group's GitHub repository, you will create a new folder called `proprietary-perspectival-dataset-creation` and within it, create a new file called `proprietary_platform_exploration.md`. In this file, you should include a link to the platform you are working with, note any barriers you discover to accessing data from the platform and consider how these might impact researchers’ ability to access and analyze data from the platform, as well as relevant screenshots or links to the terms of service. Remember this document is intended for researchers in the future who may not have access to the same platforms or data that you do, so consider what would you want them to know about experiences on these platforms.

## Part 2: Creating a Perspectival & Proprietary Dataset

Now that you have explored the challenges of accessing data from a proprietary platform, you will create a small dataset based on the data you can access. This dataset will include both data you can get from the platform and data you create collectively, allowing you to reflect on the impact of interpretation and access on datasets we collect and release from these platforms.

**Step 1: Platform Data Collection & Considerations**

Using either manual or programmatic methods, collect a small amount of data from the platform you have selected. Aim for at least one row per group member or a minimum of ten rows if you are not using a platform where the group has accounts. You should aim for at least five columns of collected data from the platform, and be sure to consider whether the data you are selecting is violating any terms of service.

**Step 2: Perspectival Data Creation**

Now that you have some initial platform data, the next step is to consider how you could add your own interpretation and subjectivity to the dataset. This perspective might look something like a column detailing the mood of a post, the genre of a song, or the relevance of a search result, but it should be something that is subjective and open to interpretation. In essence, imagine a topic that would generate discussion among your group members related to this data and try to capture that in an additional column.

Once you have decided, what your "take" on the data will be, you can generate this new data manually or programmatically (like a CLI app!), but be sure to document how you did so in your group's documentation file, outlined in the next section.

## Part 3: Reflect & Document

Once you have created your dataset, you should create documentation that reflects on the process of creating this dataset. You should create a new file called `data_reflection_documentation.md`. This file should include the following sections:

*Documenting The Data*

- [ ] Detail the structure of your dataset, including the columns you collected and the subjective column you added.
- [ ] Be sure to include the data types, as well as a brief assessment of the completeness and quality of the data.
- [ ] Include your collected and curated dataset in your folder, and be sure to anonymize any data that is proprietary or that could potentially violate terms of service.

*Reflecting on The Data*

- [ ] Reflect and detail as a group how you determined which subjective column to create and whether there was any discrepancy in how group members interpreted this column.
- [ ] Reflect and detail as a group how you determined which data to collect from the platform and whether there was any difficulties in collecting this data.
- [ ] Reflect on what platform dynamics the dataset captures and what might be difficult to detail. For example, if you are collecting data from a social media platform, how do dynamics like sponsored content or trolling impact the data?
- [ ] Reflect and detail any legal or ethical considerations that arose during the creation of this dataset, and whether these considerations impact the dataset’s use in future research if you continued to collect it.

*Licensing The Data*

- [ ] Decide as a group how you would like to license this dataset. You can use a Creative Commons license, a proprietary license, or any other license you see fit. Be sure to include this license in your group's repository. To include a license, I would highly recommend using this GitHub repository [https://github.com/santisoler/cc-licenses](https://github.com/santisoler/cc-licenses), which provides a simple way to add an image of your license which should be on your main `README.md` file in the folder and to include the license text in a `LICENSE` text file in the same folder.
- [ ] Detail any additional reuse considerations that you would like to include with your dataset.

Once you have completed this assignment, push your changes to your group's GitHub repository and be prepared to discuss your findings in class next week.

You are welcome to divide the labor between group members in whatever way you see fit, but each group member should contribute to the documentation and dataset creation process. You should also record who is responsible for each task in your GitHub project board, and remember to assign a new Project Manager.