# Proposal

!!! note
    This documenation was written in MarkDown and not on confluence, because I was able to write and proof read on a flight from Switzerland to the US.

## Abstract

The CSCS Knowledge Base (KB) is documentation that is publicly available to users of CSCS' Alps infrastructure. It is publicly available, and is written by CSCS staff - the vast majority of documentation is technical, and as such is written by CSCS engineers.

This proposal is an alternative method for the CSCS Knowledge Base, to improve the quality of the documentation, its presentation, and its maintainability.

The proposal is the result of a small team of engineers from SRM, VCUE and SSA that have been working on addressing the following issues with the KB documentation:

* improve the quality of the documentation
    * e.g. identify documentation that can be removed because it refers to deprecated systems and processes (e.g. Daint-XC)
* improve the visual presentation
* improve the layout and consistent styling of the documentation
* improve the user experience for CSCS engineers writing and maintaining KB docs.

## Problem Statement

The user-facing documentation provided by CSCS is of relatively low quality when compared to other sites, and staff are not motivated to contribute.

One of the key causes is that Confluence is not a good solution for writing and maintaining technical documenetation.
The following subsections break down the different aspects of quality.

### The visual presentation of the Confluence documentation is poor

The pages generated by Confluence lacking the visual refinement of alternatives.

There is some support for theming (and the KB currently uses a custom theme), including modifying/overriding the theme. However, it still looks and feels like Confluence. Font sizes, margins, colours are generally inconsistent and don't feel modern or readable. Larger changes are difficult because the layout is still primarily determined by what's possible with confluence. Theming is limited to what's available for Confluence.

Finally, there is not support support for small screens (e.g. phones).

!!! tip
    View this page on your phone, then load the CSCS KB docs on your phone.

    Many younger users actually use their phone for reading documentation.

### Technical documentation needs good version control and lifetime management

Confluence has history tracking on each page, however it can easily be corrupted or rewritten.
Confluence records individual page edits, but doesn't gather them together into cohesive change sets.

The following "user stories" for CSCS staff working on Confluence are the result of this:

- Spend a couple of hours editing a confluence page to have all of my changes deleted because a colleague reverted to an earlier change.
- Lose part of the page edit history when there is a reversion - we lost old documentation.

The alternative should:

* keep a complete record of the history;
* allow for tracking changes that affect more than one page as single atomic changes;
* allow proposing changes without fear of breaking existing documentation and request reviews from the "owners"
* allow external users to propose changes.

### Good documentation needs good tooling for writers

There are two target audiences for high-quality documentation:

1. the CSCS users who use the documentation;
2. the CSCS engineers who write the documentation.

The second audience is key - engineers need to be motivated and have good tools to write great documentation.

The user experience for engineers using Confluence is very poor.

!!! note
    Engineers often comment that Confluence is sold to management, not engineers, because its features don't make life easier for folk writing technical documentation.
    Every engineer that we have spoken to about this proposal has expressed enthusiasm for not using Confluence.

## Proposed Solution

We propose using the [material for mkdocs](https://xxx) framework for documentation, and deploy it using a CI/CD pipeline from GitHub.

- WS-VCUE are proposing to take responsibility for the pipeline, and tooling for helping with documentation writing

Material for MKdocs has become the gold standard for writing technical documentation.
Here are some examples of documentation written using the same framework:

- [NERSC](https://docs.nersc.gov)
- [LUMI](https://docs.lumi-supercomputer.eu/)
- [Bristol Center for Supercomputing](https://docs.isambard.ac.uk/)
- [Hummingbot](https://hummingbot.org/)
- [Material MKDocs](https://squidfunk.github.io/mkdocs-material/)

The documentation you are reading now was set up in 1 day by 1 CSCS engineer with no web development experience (@bcumming) with a simple CI/CD pipeline.
The TDS pipeline that updated the pipeline to generate a separate site for each pull request took another engineer (@andreasfink) half a day.

### The documentation

Documentation is written in Markdown, and managed in a GitHub repository.

This documentation is in a GitHub repository [bcumming/kb-poc](https://github.com/bcumming/kb-poc/), and the source for this page can be viewed [here](https://github.com/bcumming/kb-poc/blob/main/docs/rationale/index.md)

### Editing

There are two ways to edit the documentation, depending on the scope of the changes, and the need for review.

1. Small changes (fixing typos, spelling, etc) can be made directly using the text editor in the GitHub site, and merged directly without review or PR.

    !!! note
        only CSCS staff on an approved whitelist would be able to make direct changes like this.

2. Larger changes can be made made by checking out a local copy of the code, editing and reviewing the changes locally.

### Deployment

CSCS staff who contribute to the documentation would be added to a GitHub team that has write permissions to the GitHub repository
- all CSCS staff can be added, or a team of documentation gate keepers could be created.

The workflow for an individual to update the documentation would like like the following:

1. pull the documentation
    ```
    git clone git@github.com:eth-cscs/kb-docs.git
    ```
2. set up a localy copy of the documentation viewable in your browser
    ```
    cd kb-docs;
    source ./setup
    mkdocs serve
    ```
3. make changes, and check them locally in the browser
4. add changes to a new branch and push
    ```
    git switch -c small-files
    git add docs
    git commit -m 'add a user guide for handling small files in ML workloads'
    git push origin small-files
    ```
5. create a pull request on the GitHub site
6. documentation is automatically built and a "TDS link" is generated for reviewers
    - the link for each PR is unique
        - e.g. the draft documentation for PR that merged this page is here: [docs-kb.tds.cscs.ch/5/](https://docs-kb.tds.cscs.ch/5/)
7. **(optional)** request reviews from stakeholders
    - anybody with a GitHub account can be a reviewer
    - users can be asked for feedback on the draft docs.
8. merge the pull request
    - the main documentation is automatically rebuilt and redeployed by the pipeline

### Pros and Cons

**Pros**:

- the target audience, CSCS engineers, use tools that they are familiar with:
    - MarkDown for writing technical documentation
    - Git and GitHub for managing the doc
    - GitOps style workflow for review and change management
- MarkDown is far superior to the Confluence editor/"markdown"
    - it can be searched easily in an editor and command line
    - the material extensions allow for writing rich content
- higher quality generated documentation
    - on both desktop and mobile
    - the documentation can be checked by CI/CD and only docs with no broken links, missing images or accesibility issues will be merged
- better history and change management through git
    - changes can be rolled back
- single source of truth for documentation
    - the docs can be rebuilt from scratch from the markdown in a git repository
    - no storage or services required from CSCS side.
- better deployment through CI/CD
    - generate preview documentation alongside
    - it is possible to gather a "working copy" of merged updates in the TDS, and deploy using a tag.
- higher quality search
    - Confluence search is notoriously low-quality
    - the search provided the xxx framework used by Material is better (there is something incorrect here - I do not know what it is though - TODO: fix)
- the engineers responsible for writing and maintaining most of the docs are responsible for deployment
- MarkDown documentation written for other CSCS products and services can be integrated into our docs
    - for example, the `alps-uenv` recipes for uenv maintain documentation in markdown alongside the recipes
    - we can ensure that documentation is maintained alongside the product
    - there is no way to integrate those docs into our confluence KB
- features like indexes at the top of each page can't be accidentally deleted or modified by writers
- internal comments are kept in Confluence, if a page is moved (from TDS to KB) instead of copied - this looks unprofessional
- propose changes by engineers outside of their main activity area because they think claritify in the documentation is missing, and the knowledgeable engineers can refine on the proposals without interfering with the live documentation
- limited features in markdown ensure that documentation will look consistent, e.g. bullet points will all look the same across pages, code boxes will have the same style, enforced table of contents for each page, headings same style across pages, etc.

**Cons**:

- search integration with SD might be less seamless
    - while it is possible to provide higher quality search results, it might require more effort to integrate with SD.
    - the confluence+jira integration provides documentation directly inline in the SD interface
    - on the other hand, providing links to external documentation is better for bookmarking.
- The WYSIWYG editor provided by Confluence has some benefits
    - for small edits it is more convenient
    - non-technical staff can contribute more easily
- porting the existing documentation will take time
    - there are tools for converting Confluence to Markdown, but an engineer would need to proof read and check the resuls.
- MKDocs does not provide direct template support
    - it is possible to create templates and workflows around using them, but it has some maintenance cost
- there will be some effort required by CSCS engineers to maintain the infra
    - look after the TDS workflow
    - write scripts for managing templates
    - TODO: how do these required to maintaine the confluence-jira

## Alternatives

Restructured Text (RST) + Sphinx

* https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html

* shares the benefit of being a git-based workflow
* the main difference is between the syntax of RST vs MarkDown
    * Markdown is generally easier to write and read
    * RST has more features
    * It comes down to personal choice
* the default look and feel looks quite dated
    * there are [good themes available](https://sphinx-themes.org/).


## Next Steps

1. invite  key stakeholders to give feedback on the proposal
    - Service Managers
    - Working structures responsible for writing user-facing documentation
2. identify key requirements that have to be met by the proposed solution based on feedback.
3. address the - there is something missing here but I do not know what (TODO: fix)
4. make the decision **(todo: who makes the decision)**
3. **(if approved)** port the current KB docs to markdown
    - BC, AF, MS, JC offer to do a lot of heavy lifting here - WS responsible for individual pages will be required to review the results.
4. **(if approved)** deploy the documentation.