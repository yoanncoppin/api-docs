---
title: View API Changelog
---
# Overview

This page describes View API changelog.

# Change log

## 1.0-beta2
    
* new endpoint GET /api/v1/map/imagery.wmts replaces GET /mugg/wmts/{imagery}.
* endpoint GET /mugg/wmts/{imagery} is now deprecated.
    
## 1.0-beta1
    
* image _vehicle_ property has been renamed to _satellite_.
* image _id_ property has been renamed to _sourceId_.
* image _constellation_ property values have been changed (use of 'SPOT' instead of 'SPOT 6/7')
* image _sunElevation_ property has been renamed to _illuminationElevationAngle_.
* image _resolutionMeters_ property has been renamed to _resolution_.
* image _indexDate_ property has been renamed to _insertionDate_.
* _insertdtstart_ and _insertdtend_ parameters were added to _GET /api/v1/images_ endpoint to filter images based on their _insertionDate_ property.
    
    
## 1.0-alpha2
    
* Initial version


