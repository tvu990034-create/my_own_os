# Architecture Technical Corrections

## Issues Identified and Corrected

### 1. Directory Structure Correction

**Original Issue:**
- Proposed `custom/` as a sibling to `openwrt/`
- Suggested using OpenWrt as a submodule
- Created separate `feeds/` directory structure

**Problem:**
- OpenWrt build system expects to run from the `openwrt/` directory
- Feed integration expects packages to be external to the build directory
- Submodule approach would complicate local development with existing repos
- Directory structure didn't align with OpenWrt's feed mechanism

**Correction:**
- Build work happens inside `openwrt/` directory (TOPDIR)
- Custom build configuration placed in `openwrt/custom/` for build integration
- Custom packages placed in separate `custom-feed/` directory at project root
- Feeds referenced via `src-link` in `feeds.conf.local` using relative paths
- No submodules - use existing repository structure as-is

**Rationale:**
- OpenWrt's feeds script uses relative paths from TOPDIR
- Custom configuration files need to be in TOPDIR for build system access
- Feed packages should be external to avoid conflicts with core packages
- Maintains compatibility with OpenWrt's standard feed workflow

### 2. Feed Configuration Correction

**Original Issue:**
- Proposed `src-link custom-feeds custom/feeds`
- Confusing directory naming (custom/feeds vs custom-feed)

**Problem:**
- Inconsistent naming
- Would require creating nested directory structure
- Not aligned with OpenWrt feed conventions

**Correction:**
- Use `src-link custom-feed ../custom-feed`
- Single `custom-feed/` directory at project root
- Clear separation: packages in custom-feed, config in openwrt/custom

**Rationale:**
- OpenWrt feeds script supports `src-link` for local directories
- Relative path `../custom-feed` works from openwrt/ directory
- Clear separation of concerns: packages vs build config

### 3. Component Dependencies Clarification

**Original Issue:**
- Listed iproute2 as a dependency for OpenWrt

**Problem:**
- iproute2 is already included in OpenWrt packages
- Not a direct dependency of the build system
- Should be referenced as an available package, not a build dependency

**Correction:**
- iproute2 is an OpenWrt package that can be included
- Not a build-time dependency
- Will be selected via package configuration

### 4. Removed Unnecessary Complexity

**Original Issue:**
- Included build/config/scripts directories in custom/build

**Problem:**
- Too much structure for Phase 1
- Scripts may not be needed initially
- Config fragments can be added later when needed

**Correction:**
- Start with minimal structure
- Add custom/scripts only when specific scripts are needed
- Keep build/config for Kconfig fragments (future use)

### 5. Kernel Module Approach

**Original Issue:**
- Listed custom kernel modules in Phase 1

**Problem:**
- Premature for Phase 1
- No specific kernel modifications needed yet
- Should be deferred until actual requirements emerge

**Correction:**
- Keep custom/kernel directory structure but empty
- Defer kernel module development until Phase 3+ when needed
- Document as future extension point

### 6. Configuration Templates Approach

**Original Issue:**
- Listed configuration templates as Phase 1 deliverable

**Problem:**
- Templates require understanding of target hardware
- Should be created after minimal OS boots
- Cannot be validated without running system

**Correction:**
- Create directory structure but empty templates
- Defer actual template creation to Phase 2
- Focus on structure in Phase 1

### 7. Reproducible Build Configuration

**Original Issue:**
- Listed reproducible build as Phase 1 deliverable

**Problem:**
- Reproducible builds require complete configuration
- Cannot be tested without a full build
- Requires toolchain and image generation

**Correction:**
- Document reproducible build requirements
- Implement basic reproducibility checks in later phases
- Focus on feed integration validation in Phase 1

## Compatibility Considerations

### OpenWrt Feed Compatibility
- Verified: feeds script supports `src-link` for local directories
- Verified: relative paths work from TOPDIR
- Verified: feed structure follows OpenWrt conventions

### Cross-Architecture Support
- No architecture-specific code in Phase 1
- Build configuration uses OpenWrt's target system
- Custom packages are architecture-agnostic

### Windows Path Considerations
- OpenWrt requires case-sensitive filesystem
- Current Windows environment may have issues
- Document requirement for WSL or Linux build environment
- All paths documented with forward slashes

## Phase 1 Scope Adjustment

### What Phase 1 Actually Delivers
1. Custom directory structure (openwrt/custom/)
2. Custom package feed structure (custom-feed/)
3. Minimal router-core package skeleton
4. Feed configuration (feeds.conf.local)
5. Feed integration test (update and list)
6. Documentation

### What Phase 1 Does NOT Deliver
- Reproducible build (deferred to later phases)
- Configuration templates (deferred to Phase 2)
- Kernel modules (deferred to Phase 3+)
- Custom build scripts (deferred as needed)
- Actual image build (deferred to Phase 2)

## Remaining Technical Debt

1. **Build Environment**: Current Windows environment may not support OpenWrt build
   - OpenWrt requires case-sensitive filesystem
   - May need WSL or Linux VM
   - Documented in ARCHITECTURE.md

2. **Kernel Synchronization**: OpenWrt uses specific kernel versions
   - Linux repo is reference only
   - OpenWrt patches its own kernel
   - Do not directly use linux/ for builds

3. **Feed Updates**: Local feeds won't auto-update
   - src-link feeds are static
   - Manual updates required
   - Documented in workflow

## Validation Strategy

### Phase 1 Validation
1. Directory structure created
2. feeds.conf.local valid syntax
3. feeds script recognizes custom feed
4. feeds list shows custom packages
5. No syntax errors in package Makefiles

### What Cannot Be Validated in Phase 1
- Actual build (requires complete toolchain)
- Image generation (requires configuration)
- Runtime behavior (requires bootable image)

## Summary

The corrected architecture:
- Aligns with OpenWrt's build system expectations
- Uses standard feed mechanisms
- Maintains clean separation of concerns
- Provides clear extension points
- Minimizes Phase 1 scope to achievable goals
- Documents technical constraints (Windows/case-sensitivity)
