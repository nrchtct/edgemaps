# EdgeMaps

EdgeMaps is a ~~new~~ visualization technique that integrates the representation of explicit and implicit data relations. Explicit relations are specific connections between entities already present in a given dataset, while implicit relations are based on the similarity measures derived from shared properties in multidimensional data. EdgeMaps combine spatialization and graph drawing techniques to visualize both types of relations. By activating only one node at a time and distinguishing between incoming and outgoing edges, interesting visual patterns emerge resembling fireworks and waves.

**Demo:** http://mariandoerk.de/edgemaps/demo/

## Files

Simply open `index.html` in a browser. It needs no server and no build step.


| file | purpose |
|---|---|
| `index.html` | the app |
| `philosophers.js`, `painters.js`, `musicians.js` | datasets, loaded as `<script>` tags so they work over `file://` |
| `raphael-1.5.2.min.js` | Raphaël: SVG drawing |
| `jquery-1.5.2.min.js` | jQuery: DOM and events |
| `jquery.history.min.js` | jQuery History: URL-hash state |

## Data format

Each dataset file defines one global variable: `phils`, `paint` or `music`.

| key | content |
|---|---|
| `ph` | nodes, keyed by id (e.g. `immanuel_kant`) |
| `in` | interest labels |
| `to_max`, `fr_max` | largest in/out degrees for scaling node sizes |

Each node in `ph`:

| field | content |
|---|---|
| `id` | same as its key |
| `name`, `name_short` | full name, and the label shown on the map |
| `abstract` | short description for the detail panel |
| `birthyear` | string, negative for BCE, used for the timeline |
| `ints` | interests, as `{ id: 1 }` with ids from `in` |
| `to` | nodes it influenced, as `{ id: 1 }` |
| `fr` | nodes it was influenced by, as `{ id: 1 }` |
| `to_count`, `fr_count` | sizes of `to` and `fr` |
| `d1`, `d2` | MDS position in [0, 1], computed in R with `smacof` |

```js
var phils = {
  ph: {
    immanuel_kant: {
      id: "immanuel_kant", name: "Immanuel Kant", name_short: "KANT",
      abstract: "Immanuel Kant (...) was an 18th-century German philosopher ...",
      birthyear: "1724",
      ints: { ethics: 1, metaphysics: 1 },
      to: { georg_wilhelm_friedrich_hegel: 1 },
      fr: { david_hume: 1 },
      to_count: 54, fr_count: 9,
      d1: 0.7488, d2: 0.3761
    }
  },
  in: { ethics: "Ethics", metaphysics: "Metaphysics" },
  to_max: 54, fr_max: 18
};
```

To add a dataset, include its file with a `<script>` tag and add it to `DATASETS` and the `#data` menu in `index.html`.

## Changes since 2011 (2025–2026)

I have made these changes mainly as a careful restoration rather than a proper redesign. They keep the demo working in today's and hopefully tomorrow's browsers while its appearance remains consistent with the figures in the original publications.

- **URL hash state:** changes to the hash apply only what changed instead of resetting everything. Switching views keeps the search and the selected node. 
- **Search:** user input is escaped, so characters like `(` or `+` no longer break it. Name, abstract and interest matches are combined.
- **Rendering:** sizes scale with the window. Small nodes are drawn on top so they stay clickable. Hidden timeline labels and the legend no longer block clicks on the background, and clicking an edge also cancels the selection. Animations are a bit faster.
- **Detail panel:** links to Wikipedia. Freebase images and links were removed because the service has shut down for a while.
- **Data files:** one `DATASETS` table instead of `switch` statements. The timeline's year range is computed from the birth years. Removed the `/en/` prefix from ids and dropped unused fields (`guid`, `img_guid`, `am`, `yr_min`, `yr_max`).
- **Cleanup:** removed dead code and old browser hacks.
