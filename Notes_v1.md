
# Overview

This repository is used to test the content creation interface for a simple travel blog. Content is added by creators using different templates for Github Issues. The creation of these Issues triggers workflows which commit the content contained in the Issue to the repository, to be displayed on the website upon deployment.

# Use of Github Issues

The locations.json file specifies the locations for the trip:
- Each location has a name, latitude, longitude
- New locations are added by creating a Github Issue with a custom template (location.yml) allowing the above three fields
- Creation of this specific issue type triggers a workflow which updates the locations.json and the post.yml template descibed below

A post is created by adding a new Github Issues using the post.yml template:
- This template allows a title, text and a list of image URLs
- The list of available locations is set by the above mentioned workflow
- Creation of this specific issue type triggers a workflow which creates a post .json under the pots/ directory
- This then triggers a workflow which creates the tip.json file descibed below

The trip.json file is the final combined data source used as content for the blog:
- It is generated from scratch by the above mentioned workflow by slotting in each post into their respetive locations (under a list of `posts`)
- Locations with no posts is allowed and expected (ie. the `posts` list is simply `[]`)

# Workflows:

## Overall pipeline for creating a new location:

New location issue 
    |
    | (Workflow triggered on all Issues, then filtered by type)
    v
1. Actions updates locations.json 
2. Actions updates trip.json

## Overall pipeline for creating a new post:

New post Issue
    |
    | (Workflow triggered on all Issues, then filtered by type)
    v
1. Actions creates new .json under posts/
2. Actions updates trip.json
