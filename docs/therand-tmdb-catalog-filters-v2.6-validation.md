# Therand TMDb catalogue filters — validated V2.6 checkpoint

Validated on Kodi 21.3 / LibreELEC on 2026-08-26 against JackTook upstream commit `bf0fe656dde4ea45f33a9ca99f4bbdeba28f8d43`.

## Device installer

`install-jacktook-catalog-filters-v2.6-cursor-recent-grace.sh`

SHA-256: `b555a0540490549f974bcc5299a0d97a862434d06eac26d099be492f3d7ada58`

This file records the validated device behaviour before the implementation is ported cleanly into the branch. The installer itself is a deployment/test vehicle and is not intended for the upstream PR.

## Validated behaviour

- optional filtering of movies not yet released using TMDb release details;
- theatrical types 2/3 count as released; digital type 4 counts only when no theatrical release is present in relevant markets; premiere/festival, physical and TV dates do not count;
- optional TMDb adult flag filtering;
- optional obscure explicit-content heuristic with documentary-aware handling;
- optional hide-movies-without-poster filter;
- optional hide-movies-without-background (`backdrop_path`) filter;
- smart rescue for notable titles whose original language is excluded;
- optional treatment of valid TMDb original-language codes absent from JackTook's native picker as excluded, while retaining smart rescue and explicit browse-language force-allow;
- optional very-obscure-production filter with stronger documentary thresholds;
- recent-release grace window for the obscurity filter (validated at 180 days);
- manual TMDb search override can include catalogue-filtered titles;
- compact 20-item logical pagination;
- identity dedupe based primarily on media type + TMDb ID;
- cursor-based continuation between logical pages, avoiding cumulative rescans from raw TMDb page 1.

## Device observations used for validation

- `XXXXXXX` (TMDb 457697) is removed by the documentary obscurity rule;
- the previously visible Urdu-language documentary is removed by the unlisted-language rule;
- `Le Bon, la Brute et le Truand` remains visible through smart notability rescue when Italian is excluded;
- `Orang-outan` remains visible after adding the recent-release grace window;
- logical page 2 resumes from the previous compact cursor instead of rescanning the full prefix.

## Upstream-port requirements

Before opening an upstream PR:

1. port the validated logic as normal source changes, not as an installer/patch script;
2. keep release-market defaults neutral/generic — do **not** hard-code `BE,FR` upstream;
3. add unit tests for release semantics, smart language rescue, unlisted languages, explicit/documentary filtering, missing poster/backdrop, search bypass, dedupe, recent-release grace and cursor pagination;
4. preserve explicit browse-by-language `force_allow_lang` semantics;
5. keep the upstream default behaviour unchanged when all new filters are disabled;
6. run CI and perform one final Kodi validation build before proposing the PR.
