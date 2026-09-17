# Ground-up learning redesign

Specification: `attalambda-site-ground-up-learning-redesign-spec.md`, supplied
2026-09-17. Original endpoint: implemented, tested, reviewed candidate.
After acceptance, Kyle explicitly authorized committing, pushing, and publishing
on 2026-09-17. Git destination: `origin/main` in `kserrec/attalambda-site`.

## Verified starting state

- Branch: `main`; HEAD and remote main:
  `46263fec9e8478b079aeadd7e32d8981c09fbbed`. Clean worktree, no open site PRs.
- Existing production files: four HTML pages and `style.css`; `LICENSE` stays
  unchanged. Add `learn.html`; this implementation record is the only intended
  additional non-production file. No language repository changes.
- Latest public release: `v0.8.0`, source
  `f309199baa170ba5b12ff6b18b60dc49c114a8a1`.
- Newer language main inspected:
  `f6938b286329532230be210de0eaaa398286d38a`; differences are documentation only.
- Temporary evidence, source excerpts, downloads, programs, browser profile,
  screenshots and scripts: `/tmp/attalambda-redesign/`, outside this repository.
- Spec references to “six pages” mean five HTML pages plus the shared CSS file.

## Release teaching truth sheet

All paths below refer to the release source, not newer main.

- `macros/macros.rkt`: `def` emits a `define` with nested unary lambdas.
  `lang/expander.rkt` curries applications/lambdas, lowers sequential `let`,
  `list`, and `cond`; rejects ordinary recursive module bindings.
- `core/logic.rkt`: raw true/false choose their first/second arguments.
  `core/typed-logic.rkt`: public TRUE/FALSE wrap those selectors with Bool tag.
- `core/pair.rkt`: `raw-pair left right selector = ((selector left) right)`.
  `core/objects.rkt`: object = pair of tag and payload; no host struct.
- `core/tags.rkt`: Church tags 0 Error, 1 Bool, 2 List, 3 unassigned to a
  current public type, 4 Result, 5 Char, 6 String, 7 Rat, 8 Unit, 9 Byte,
  10 Option, 11 Map. Error kinds/positions also use Church metadata.
- `core/rat.rkt`, `core/int.rkt`, `core/binary-nat.rkt`, literal expansion:
  Rat payload pairs a signed numerator with a positive denominator, reduced;
  numerator pairs raw Boolean sign with binary magnitude. Magnitudes are
  lambda-encoded Lists of raw Boolean bits, not Church arithmetic or host ints.
- `core/errors.rkt`, `core/lists.rkt`: nonempty List = tagged pair(head, tail).
  Canonical NIL has empty-list-error in both payload positions. NIL and that
  Error's empty frame list are tied together through a fixed point.
- `core/option.rkt`: Option payload = pair(raw is-some Bool, payload);
  NONE uses raw-false for both. Public some propagates an Error argument.
- `core/result.rkt`: Result payload = pair(raw success Bool, payload).
  make-ok propagates Errors; make-err deliberately consumes an Error as data.
- `core/errors.rkt`, `core/typecheck.rkt`: Error payload = pair(root, frames);
  root = pair(kind, details); mismatch details = pair(position,
  pair(expected, actual)); frame = pair(function-name, pair(position, expected)).
  New frames are prepended; rendering reverses them to show the origin first.
- `core/fix.rkt`, `lang/expander.rkt`: rec gives a function its recursive self
  via language-fix (imported raw-fix). Fixed point is built from self-application;
  lazy evaluation matters. Describe as a fixed-point combinator.
- `runtime/host.rkt`, `runtime/codec.rkt`: host validates/converts encoded
  requests and handles stdout, read-line, files, TCP, exit. Pure core arithmetic
  and data operations remain lambda computation; host conversion is a boundary.

## Phases and steps

- [x] Phase 1: 1.1 workspace, 1.2 release identity, 1.3 source truth sheet,
  1.4 artifact download/checksum/version/help/file/transcript/interactive probes.
- [x] Phase 2 implementation: 2.1 Learn shell/anchors, 2.2 navigation,
  2.3 page roles. Parser and ordinary HTTP checks pass.
- [x] Checkpoint 2 browser gate: all five pages opened through file:// and
  HTTP in real Chrome; all 50 local header-navigation paths passed.
- [x] Phase 3: 3.1 foundation/reduction, 3.2 currying/def, 3.3 raw Bool,
  3.4 real pairs. Fresh prerequisite-order review passed.
- [x] Phase 4: 4.1 actual objects, 4.2 structural notation, 4.3 semantic
  notation, 4.4 minimal styles. Fresh abstraction-integrity review passed.
- [x] Phase 5: 5.1 public Bool, 5.2 runtime contracts, 5.3 Church tags,
  5.4 practical Rats, 5.5 List, 5.6 Option, 5.7 Result, 5.8 Error.
  Source comparison and real mismatch/nested-mismatch execution passed.
- [x] Phase 6: 6.1 recursion problem, 6.2 fixed point, 6.3 host, 6.4 synthesis.
  Main lesson reviewed with disclosures excluded; core path stands alone.
- [x] Phase 7: 7.1 concise Home, 7.2 Language overview, 7.3 Examples,
  7.4 current download/shell, 7.5 stale-copy removal. Whole-site content
  coherence reviewed; final HTML download instructions executed cleanly.
- [x] 8.1 HTML/CSS simplicity review; no new dependency or external resource.
- [x] 8.2 real-browser 320px and 390px layouts.
- [x] 8.3 real-browser desktop at 1440px and actual 200% browser zoom.
- [x] 8.4 real-browser keyboard, visible focus, all four disclosures, all
  17 Learn fragments, and horizontally scrolling code blocks.
- [x] 8.5 all final complete programs and displayed outputs executed.
- [x] 8.6 all local links and fragment targets checked.
- [x] 8.7 all 30 unique external links reachable (HTTP 200).
- [x] 8.8 fresh content/source/simplicity review. One CSS selector concern
  corrected and independently rechecked; no unresolved content findings.
- [x] 8.9 working-tree identity and repository hygiene recorded.
- [x] Checkpoint 8: final candidate gate passed. The six production files
  still match the recorded hashes; all earlier execution/source evidence remains valid.

Implementation continued through source-based work after the browser gate was
blocked, rather than treating unavailable browser evidence as a pass. This is
an explicit execution-order deviation. Browser authorization was subsequently
confirmed and all browser acceptance criteria were completed, as recorded below.

## Verification evidence

- `git status --short --branch`, `git rev-parse HEAD`, `git remote -v`;
  GitHub API latest release, site main, open PRs, language main queries.
- Release source read with `git show v0.8.0:<explicit path>`; newer main
  compared using `git diff v0.8.0 f6938b2 --stat` with dotenv exclusions.
- Public Linux archive and SHA256SUMS fetched via `curl -fL`; in an empty
  temporary directory, `sha256sum -c SHA256SUMS` reports OK. Archive SHA256:
  `f1b8b49ba659485e089ffcf38d1d5999016131de65e21177bb922fa86013d3fc`.
- `tar -xzf` extraction; binary `--version` → `AttaLambda 0.8.0\n`;
  `--help` shows file, --repl, --no-history; included hello →
  `Hello from AttaLambda.\n`.
- Temporary programs: mismatch → `ERROR(add(arg2 expected RAT got STRING))`;
  nested mismatch → `ERROR(add(arg2 expected RAT got STRING)\n  -> mult(arg2 expected RAT))`.
  Both exit 0, no trailing newline, no stderr.
- `--repl --no-history` with fractional addition, saved twice function and
  `(twice 21)` → `=> 1/2\n=> 42\n`; :help on stderr; :quit exits 0.
- Automated pseudo-terminal test of `--no-history` observed the banner,
  `atta> ` prompts, `=> 1/2`, silent definition, `=> 42`, and normal :quit exit.
  Raw transcript: `/tmp/attalambda-redesign/interactive-pty.txt`.
- `python3 /tmp/attalambda-redesign/check-site.py` extracted and decoded all
  complete programs from final HTML, wrote temporary .attl files, ran the
  public binary, and compared exact stdout bytes. All 10 passed, all exit 0,
  no stderr. Shell input extracted from Get started also matched exact output.
- That temporary parser checked 88 local links/fragments across all five HTML
  pages, unique IDs, heading order, one main and h1 per page, consistent
  navigation/order/current-page labels, and named keyboard-focusable code
  blocks. All passed. No scripts or event-handler attributes were found.
- `python3 -m http.server 8765 --bind 127.0.0.1` served all five pages with
  HTTP 200 and exact file bytes; no browser behavior is inferred from this.
- `python3 /tmp/attalambda-redesign/check-external.py` performed HTTP HEAD
  checks with redirects for all 30 unique external links; every target returned
  200, including pinned sources, release assets, example links and Rojas PDF.
- `python3 /tmp/attalambda-redesign/check-install.py` fetched both links from
  final Get started HTML into a new empty temporary directory. It executed
  the published checksum/extraction/version/included-example/custom-program
  commands and compared every displayed output exactly. The same fresh
  artifact passed the final shell transcript. Extraction used TAR_OPTIONS
  exclusions for opaque dotenv files; no dotenv contents were accessed.
- `racket /tmp/attalambda-redesign/probe-foundations.rkt` applied the actual
  pinned macros, Boolean functions, pair/object selectors, Church numeral 7,
  and fixed-point implementation. Forced results matched
  `(apple apple orange A B payload yes 7 120)` exactly. The initial probe
  printed unevaluated promises; only the temporary probe was corrected to
  force its list and compare actual values.
- Two fresh read-only reviewers compared content to release source. The first
  correctly reconstructed the selector lambda and binary Rat payload from the
  semantic object drawing, and found no prerequisite-order/content issue.
  The final reviewer found no content inaccuracies or scope creep. It spotted
  that `nav a { white-space: nowrap }` also affected the long new Learn TOC;
  this was scoped to `header nav a`, and the reviewer verified the correction.
  No rendered mobile result is claimed from this source-level correction.
- `git diff --check` passes. The new Learn file was separately checked with
  `git diff --no-index --check /dev/null learn.html` (exit 1 denotes a new-file
  diff, with no whitespace diagnostics) and a trailing-whitespace scan.
  All six production files contain no 0.7.0 or obsolete source-commit reference.

### Final program outputs

All ten outputs have no trailing newline. Duplicate examples were run from
each final HTML occurrence, not inferred from another page's result.

- Home list mapping: `[1, 4, 9, 16]`.
- Learn exact addition: `1/2`.
- Learn mismatch: `ERROR(add(arg2 expected RAT got STRING))`.
- Learn nested mismatch: `ERROR(add(arg2 expected RAT got STRING)\n  -> mult(arg2 expected RAT))`.
- Learn factorial: `120`.
- Examples hello: `"Hello, world."` (quotation marks included).
- Examples fraction: `1/2`.
- Examples mapping: `[1, 4, 9, 16]`.
- Examples factorial: `120`.
- Get started custom hello: `"Hello, world."` (quotation marks included).
- Included distribution hello (separate workflow check):
  `Hello from AttaLambda.\n`.
- Shell results: `=> 1/2\n=> 42\n`.

Here `\n` records an actual newline. Full temporary evidence is in
`site-checks.json`, `external-checks.json`, `install-checks.json`, and
`foundations-output.txt` under `/tmp/attalambda-redesign/`.

## Browser verification and resolved access issue

The first Chrome launch inside the shell sandbox failed because its sandbox
helper could not run there. The escalation tool then returned `rejected by
user`; that message did not establish that Kyle personally rejected it. Kyle
explicitly clarified that he had not rejected anything and approved browser
verification. Retrying the Chrome launch with that authorization succeeded.
No browser safeguards or system security settings were changed.

Real browser: Chrome 153.0.8010.47, isolated temporary profile at
`/tmp/attalambda-redesign/chrome`, remote debugging restricted to localhost.
One-off browser checks use Node's built-in WebSocket and Chrome's DevTools
Protocol; no package or site JavaScript was added.

- `node /tmp/attalambda-redesign/check-browser.mjs`: all five pages at 320px,
  390px, and 1440px (15 layout cases), each with disclosures closed and open.
  No document-wide horizontal overflow, clipped links/disclosure controls,
  clipped tables or code-block containers. Long code scrolls within its block.
- Same check opened each page from disk and over HTTP, verifying stylesheet
  application and following all five local header links from each starting
  page under both schemes: 50 navigation paths passed.
- `python3 /tmp/attalambda-redesign/browser-zoom.py --200` sent native Chrome
  zoom keyboard shortcuts on the isolated display. Device emulation was
  cleared first. The real window stayed 1440px wide while the content viewport
  became 720 CSS pixels, devicePixelRatio became 2, and visualViewport.scale
  stayed 1: actual browser zoom, not pinch zoom or a CSS transform.
- `node /tmp/attalambda-redesign/check-zoom.mjs`: all five pages passed at
  that native 200% zoom, both disclosure states, including keyboard focus.
  Zoom was restored to 100% afterward.
- `node /tmp/attalambda-redesign/check-keyboard.mjs`: Tab traversed 183
  visible links/code blocks/disclosure controls across all five pages with
  the expected visible 2px red outline. Enter opened each of four native
  disclosures and Space closed it. Another nine controls inside open
  disclosures entered the Tab order and retained visible focus.
- Enter activated all 17 Learn contents links, with correct URL fragments
  and heading positions inside the viewport. ArrowRight scrolled a long
  focused code block by 20px while page scrollX stayed 0.
- Screenshots were visually inspected for desktop Home, desktop/mobile object
  derivation, mobile contents, the tag table, Language, the interactive-shell
  instructions, keyboard code focus, and the real 200% view. Text, wrapping,
  code scrolling, tables and sparse visual identity are readable and intact.
- Temporary test-harness issues were diagnosed without changing the site:
  keyboard events sent before the first rendered frames could be dropped;
  waiting for animation frames produced 10/10 delivered events. Enter events
  needed their text/keypress component to activate native details; event logs
  proved the omitted keypress and then the correct click. Closed details can
  retain client rectangles while being invisible, so the expected Tab-order
  list uses the browser's checkVisibility() result. Final checks pass with
  those test-harness corrections; no production correction was required.

Detailed evidence: `browser-layout-checks.json`, `browser-zoom-checks.json`,
`browser-keyboard-checks.json`, and PNG screenshots under
`/tmp/attalambda-redesign/`. All browser requirements are now verified.

## Delivery identity and cleanup

The verified candidate was prepared on `main` from
`46263fec9e8478b079aeadd7e32d8981c09fbbed`. Its six production files are
identified by the SHA-256 hashes below; publication uses these exact inputs.

Production changes: new `learn.html`; modified `index.html`, `language.html`,
`examples.html`, `get-started.html`, `style.css`. This `PLAN.md` is the sole
additional documentation file, justified by the required durable record.
No permanent test files or runtime code changed. `LICENSE` is unchanged;
there were no unrelated starting changes to preserve. Home and Examples keep
their original runnable program behavior, reverified on 0.8.0.

At final read-only status, the separate language repository reports modified
`HANDOFF.md` and `PLAN.md`. This task made no writes there; those concurrent
documentation changes were left untouched. Teaching evidence remains pinned
to the release commit rather than that working tree.

- `index.html`: `3bdf4f4eed313b0fd1dce89a26c83a21c01f1c073c853fa35ce850c8ff5cb909`
- `learn.html`: `7938b74b41d3b710ed809f0a525a720cb31e698685defa258634c5fff9992339`
- `language.html`: `b8e43ce051c7bc1374e147598186536f9dae368b1558f0f781454c11c1485494`
- `examples.html`: `dbc0244a411b73b87de8a8cbbe7cea27bab5b4345fc730026d4ced2a22f3a7cd`
- `get-started.html`: `5924bdd18fbb790c707f8b232d82ec1e0239c1a5b71f011ff2ad3d54ae9782ba`
- `style.css`: `4df13cc214a5790cabf8aa335baf2f099a157b6aedcf6f8bece2e239c843425b`

All downloaded archives, extracted distributions, temporary source excerpts,
programs, scripts, and browser-profile files are outside the repository under
`/tmp/attalambda-redesign/`. Browser screenshots also remain there; no
generated asset entered the repository. The temporary browser and HTTP server
were closed after verification.

## Delivery in progress

Implemented, tested, and reviewed: yes. All candidate acceptance gates passed.
Kyle has authorized commit, push, and publication. The verified destination is
`origin/main`; it is the repository's only branch, has no protection rules,
and still points to the starting revision. No pull-request workflow is defined.

GitHub Pages, Actions workflows, deployments, repository environments, and a
homepage URL were absent at the publication check. The initial commit also
records hosting as outside its prior scope. Publication destination is awaiting
Kyle's answer; this does not block committing and pushing the verified site.
Final publication evidence will be recorded after the destination is confirmed.
