# Changelog

**IQ Bible API Official Documentation**  
All notable changes to this project will be documented in this file.

We follow [Semantic Versioning](https://semver.org/), with a clarification:

- **MAJOR.MINOR** versions (e.g., `1.38`) track the corresponding IQ Bible API version this documentation is aligned with.
- **PATCH** version (e.g., `1.38.2`) reflects changes to the documentation only — not the API itself.

This allows the docs to evolve while clearly indicating which version of the API they describe.

## Unreleased
- n/a

## [1.38.7] - 2026-08-28
### Changed
- Retitled to "IQ Bible API v1 — Official Documentation"
### Added
- Notice at the top of the README: this documents v1 (RapidAPI); v1 is stable
  but no longer receiving new features; new projects should use IQ Bible API v2
  at developer.iqbible.com

## [1.38.6] - 2025-08-14
### Fixed
- Incorrect curl request example under Getting Started

## [1.38.5] - 2025-08-06
### Added
- Endpoint groupings and styles
### Changed
- Moved repo to IQ Bible org (q.v.)
- Updated authorship section
- Moved categories.md to _archive (ignored)

## [1.38.4] - 2025-08-01
### Changed
- Edited instructions for 'How do I get an API key?' (RapidAPI)

## [1.38.3] - 2025-07-31
### Added
- humans.txt

## [1.38.2] - 2025-07-31
### Fixed
- Incorrect version (niv) listed in documentation under Common Parameters > versionId
### Added
- Note about extra-biblical bookId and chapterId usage under Common Parameters
- Authorship section
### Changed
- Endpoints introduction to convey that all endpoints are currently Get requests
### Removed
- Subsection usage of --- (for sections only)

## [1.38.1] - 2025-07-31
### Changed
- Reformatted CHANGELOG
- Get Requests title to Endpoints for clarity and to allow future additions of POST, PUT, etc.
### Added
- Added Common Parameters section
- Markdown h1 title: # IQ Bible API Official Documentation

## [1.38.0] - 2025-07-30
### Added
- Repo setup
- Reformatted markdown headers to make way for Welcome, Getting Started, and FAQ sections.
- Changed email info to info@iqbible.com
- 'develop' branch created
