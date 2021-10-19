# Best Practices Contribution Guide

## Proposing updates or requesting changes

Open a [list of issues](https://gitlab.veeam.com/veeambp/vbprod/-/issues) and use **New issue** button to [create a new issue](https://gitlab.veeam.com/veeambp/vbprod/-/issues/new).

Enter meaningful title that describe the proposed changes and use issue description field to provide any additional relevant information, i. e. suggested wording, links to forums or documentation, references to field cases, etc.

The rest of the issue fields can be left empty. You can attach relevant labels to the newly created issue but it is not mandatory, it is fine to not use labels if you are unsure, project maintainers will adjust the issue information later on if necessary.

> **Note:** It is a good idea to look through a list of outstanding issues and check if similar topic was already suggested and discussed before creating a new issue. Search option may be useful in finding previously submitted issues using keywords.

## How to contribute content

The [list of issues](https://gitlab.veeam.com/veeambp/vbprod/-/issues) is the primary source of information that in the end lands in the BPG. Updates to BPG may come in different flavors, ranging from writing completely new sections or updating existing content to fixing typos. Adding relevant references and field observations in the comments is valuable contribution too.

In order to contribute start with going to the list of issues and looking through the outstanding issues. Depending on the level of your confidence you may start with just commenting on existing issues or go straight to taking ownership over some issue.

The issues that need to be paid attention first are those marked with ["needs author" label](https://gitlab.veeam.com/veeambp/vbprod/-/issues?label_name%5B%5D=needs+author) or issues [without an owner](https://gitlab.veeam.com/veeambp/vbprod/-/issues?scope=all&utf8=%E2%9C%93&state=opened&assignee_id=None).

Issues that already have owner and are being worked on should have open merge requests that can also be reviewed and commented on, you can take a look at those too.

## Submitting content

Make sure there is an outstanding issue relevant to the topic. Create an issue if necessary. Using issues allows for defining the scope before spending time on the content, discuss the content with the peers to shape it further, and benefit from advanced branching and workflow features.

Take the ownership of the issue by assigning it to yourself (use "assign myself" link in the right sidebar).

Create a merge request for the issue. Creating merge request will additionally create a new branch dedicated for this specific task. Branching out for each piece of content helps with isolating work and easier merge to upstream if the content requires updating different sections of the guide. Merge requests help with the peer review when the content is ready.

> Note: Not all issues may need a merge request. As an example content reviews do not necessarily have to end in updating the content. Creating merge requests is justified only when the content has to be changed in order to successfully close the issue, while for example "review content" issues may end in simply confirming that the specified content is still up-to-date or such issues may lead to creating separate issues dedicated to specific topics.

Work on the content - write new or update or validate depending on the nature of the task using the corresponding branch. If you cannot find the branch, navigate to the merge request and click branch URL linked below the merge request description in the "Request to merge **BRANCHNAME** into dev" block.

Files and folders should be organized as described in the [File & Folder Structure](#file-folder-structure) section. Markdown files should include frontmatter headers according to the [Page Headers](#page-headers) section.

When editing the content offline on your computer by pulling the repository, make sure you work with the correct branch of the local repository.

When done with adding/editing the content go to merge request and click **Mark as ready** button to indicate that the work is done and the merge request is ready for review and merging upstream.

## Reviewing and approving content

Before merging the content to upstream branch and making the updates publicly available the containing merge request has to be reviewed and approved.

Review phase is necessary to ensure that the content is ready for being published including formatting, is factually correct and does not contain stupid typos or other mistakes. During review the page headers should also be checked to make sure the content fits the general structure of the guide.

Merge requests not yet fully approved have "N left" marking in the [list of MRs that are ready to merge](https://gitlab.veeam.com/veeambp/vbprod/-/merge_requests?scope=all&utf8=%E2%9C%93&state=opened&draft=no).

When you are done with the review and want to approve the MR for merge, use the "Approve" button in the top section of the corresponding MR. 

---

## File & Folder Structure

For a TOC like this

* Anatomy
    * vSphere
        * Interaction
        * Transport Modes            
    * Hyper-V
        * Interaction
    * Backups 

The folder structure should look like this

    anatomy/
    |-- vsphere/
    |   |-- interaction.md
    |   |-- transport_modes.md
    |   |-- media/
    |       |-- transport_mode_whatever_is_shown.png
    |       |-- transport_mode_image_name2.png        
    |       |-- interaction_image_name.png
    |-- hyperv/
    |   |-- interaction.md
    |-- backups.md
    |-- media/
        |-- backups_321.png
     
## Page Headers

The BP will be based on a [Just-The-Docs](https://pmarsceill.github.io/just-the-docs/) 
template, so every page needs a specific header to be automatically included in
the ToC and navigation. Check out [Navigtion Structure](https://pmarsceill.github.io/just-the-docs/docs/navigation-structure/)
section for details.

### Examples

Some examples which can be copy & pasted and adapted:

**Root Section** (`/anatomy/`)

    ---
    title: Anatomy
    nav_order: 20
    has_toc: false
    has_children: true
    permalink: /anatomy/
    ---

**Page in root-section** (`/anatomy/interaction.md`)

    ---
    title: Guest Interaction
    parent: Anatomy
    nav_order: 60
    permalink: /anatomy/guestinteraction/
    ---
    
**Sub-Section** (`/anatomy/hyper-v/index.md`)

    ---
    title: Hyper-V
    parent: Anatomy
    nav_order: 30
    has_toc: false
    has_children: true
    permalink: /anatomy/hyper-v/
    ---
    
**Page in sub-section** (`/anatomy/hyper-v/instantvmrecovery.md`)

    ---
    title: Instant VM Recovery
    parent: Hyper-V
    grand_parent: Anatomy
    nav_order: 60
    ---

## Redirects

When moving pages or content it might be required to use a redirect from the old page to the new one. For this we're using the [jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from) plug-in.

A page `A.md` is reachable at `https://bp.veeam.com/vbr/A.html`. Now the page should be moved to a subsection `sub`, so the file is moved to `sub/A.md` in the repository.
The corresponding link would change to `https://bp.veeam.com/vbr/sub/A.html` and the previous link would result in an HTTP-404 error as the page is not existant anymore.

Adding this configuration to the header of `sub/A.md` will result in `/A.html` being created when compiling the page and redirecting (HTTP-301) to `sub/A.html`, so that both links are valid.

   ---
    title: A
    parent: Sub    
    nav_order: 10
    redirect_from: /A.html
    --- 

You can also specify multiple redirect sources:

    redirect_from:
    - /A.html
    - /another_page.html
    - /a_folder/

You can also use a `redirect_to` directive, but this should be only used in very seldom cases to keep the repository clean of "dummy files"