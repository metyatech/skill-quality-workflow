# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed

- Clarified that quality-workflow keeps `AC -> evidence` tracking internally
  for every state-changing task, while normal user-facing final reports stay
  concise unless the user requests detailed proof or higher-priority rules
  require path-specific evidence
- Added explicit external contract surface verification guidance so agents
  enumerate changed user-visible surfaces, build a small surface matrix, and
  include direct checks for discoverability surfaces such as CLI help, usage,
  version, schema, and equivalent documented entry points

## [0.1.0] - Initial release
