# Triage: Or, How I Finally Got Away With Haskell in Anger

Slides for the talk at [MuniHac 2026](https://munihac.de), Munich, October 10, 2026.

**[Open the slides in your browser](https://twla-io.github.io/munihac-2026-triage-slides/talk.html)**

Niko Pachuashvili · [Twla](https://twla.io) · niko@twla.io · [@nikolozp.bsky.social](https://bsky.app/profile/nikolozp.bsky.social)

The code from the talk: [github.com/twla-io/triage](https://github.com/twla-io/triage)

## About the talk

One evening, a doctor called me for help. Her practice needed a patient scheduling platform.

The talk has three parts.

**A tangled path to functional programming.** From OOP design patterns, through F# and Clojure, to Haskell. Haskell taught me a way of computing, and that way then worked in any language. If you are taking your first steps and still hesitating, this part is for you.

**Triage.** I wrote the domain model by hand, in one Haskell module: `Domain.hs`. The weak parts of the practice became strong types. But the doctor needed working software, and I couldn't build a whole application alone. So I tried an experiment: `Domain.hs` became the specification, and Claude Code generated every other layer, from the database schema to the UI.

*The Smart Constructor Found the Bug.* The doctor's calendar is sealed: its smart constructor, `mkDoctorCalendar`, rejects overlapping entries of the same doctor. A fresh agent rebuilt the persistence layer from `Domain.hs` and the skills alone, and proposed a rule: one snapshot per read. Why? Loading a calendar took two queries. A booking committed between them, and the application assembled a calendar that never existed: the same doctor, the same half-hour, twice. `mkDoctorCalendar` would reject it. The earlier AI-generated code had this race. Not a test, not a user found it. A type did. The fix was small: both queries in one REPEATABLE READ transaction. Now it is a rule in the skill.

Then the final experiment. I deleted every generated layer, and fresh agents regenerated them from `Domain.hs` and the skills alone. The tests passed, and the API was unchanged. Four differences became four new rules.

**How do we know it's correct?** The honest answer: I don't know. Generated code needs review and tests, as hand-written code would. So the talk walks through the chain of checks, layer by layer, and what they can't catch.

## Files

- `talk.md`: the deck, in [Marp](https://marp.app/) Markdown
- `talk.html`: the deck rendered with Marp CLI, served by GitHub Pages
- `images/`: images used on the slides

## Render

```sh
npx @marp-team/marp-cli talk.md --allow-local-files -o talk.html
npx @marp-team/marp-cli talk.md --allow-local-files -o talk.pdf
```

## License

© 2026 Niko Pachuashvili. The slides are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); see [LICENSE](LICENSE).

Not covered: the book covers (`images/haskell-school-of-music.jpg`, `images/learn-you-a-haskell.jpg`, `images/programming-in-haskell.jpg`), the tweet screenshot (`images/hutton-tweet.jpg`) and the Twla logo (`images/twla-logo.png`) belong to their respective owners.
