# Tipu
Tipu - Universal Level 4 Annotation for High Resolution Metabolomics

## Introduction

Annotation is the process of assigning identifiers to features observed in mass spectrometry data. Tipu solves a modified form of the change counting problem in order to generate level 4 annotations to metabolomics datasets. Tipu makes several assumptions to make this calculation possible, namely that the set of elements to be considered is known apriori, that the mass spectrometer is tuned correctly to a given mass accuracy provided in PPM, and that for every metabolite in an acquisition, its most dominant isotopologue form is present. Other less abundant isotopologues may also be present, but Tipu is not attempting to solve their annotation at this time. 

## Usage

Tipu queries consist of a floating point value which is the observed m/z value of the feature and its assumed or calculated mass accuracy in ppm. By default Tipu uses an exhaustive set of elements that never needs to be changed except when searching for exceptionally rare elements in biological systems. The return value is a list of possible formulas for that query and metrics to aid in the selection of the best match. Tipu returns only monoisotopic formulas, so isotopologues need to be inferred and annotated as such using an alterantive pre-processing tool such as Khipu.

## Installation

Install Tipu with pip:

`pip install tipu-metabolomics`

Or install from source.

## Design

Given a target value, e.g., an m/z or mass value, this can be formulated as a change counting problem where each unit of change is an element and its mass. Thus we need to simply find all combinations of possible coins (elemements) that sum to the target value. This change counting problem is an NP-Hard problem with no good solutions though, making calculation difficult. Instead, we can introduce a bit of fuzziness or approximation to make the problem no longer NP-Hard. By explicitly modeling the accuracy of the instrument, we can approximate this coin-change problem using dynamic programming in polynomial time, making this calculation tractable. 

The result of this optimization is that Tipu is very fast, but gets slower for more complex queries or larger target values. We can mitigate this through databases by observing that for a given query mass and ppm, the set of possible answers is finite and immutable if the element list is fixed. This database is stored as a sqlite file in local mode or with our cloud hosted database instead. Queries are cached in this database and grown over time with new queries. A hybrid mode where a local database is populated for offline use and optionally allows you to report your findings back to the central could database at your discretion.

## 

