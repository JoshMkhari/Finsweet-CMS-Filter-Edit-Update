# CMS Filter with AND Functionality

This repository contains a modified version of the CMS Filter attribute provided by Finsweet. The original CMS Filter attribute allows for creating advanced and complex no-code filter systems inside Webflow CMS. However, the default behavior of the filter is to use an "OR" condition, meaning it shows items that match any of the selected filter criteria.

In this modified version, the filtering logic has been updated to use an "AND" condition instead. This means that the filter will only show items that match all of the selected filter criteria simultaneously.

## Original CMS Filter

The original CMS Filter attribute can be found in the Finsweet Attributes repository on npm:

- Package: [@finsweet/attributes-cmsfilter](https://www.npmjs.com/package/@finsweet/attributes-cmsfilter?activeTab=code)
- Documentation: [Finsweet CMS Filter Documentation](https://finsweet.com/attributes/cms-filter#documentation)

## Modifications

To achieve the "AND" functionality, the following modifications were made to the original CMS Filter JavaScript code:

1. The `Ur` function, which is responsible for filtering the items based on the selected filter criteria, has been modified. The changes can be found in the following lines of code:

   ```javascript
   // Original code code Line 885
    Ur = (t, {
            filterKeys: e,
            values: r,
            match: n,
            mode: o,
            highlight: s,
            highlightCSSClass: i,
            elements: l
        }) => {
            let a = [...r];
            if (!a.length) return !0;
            let m = e.includes("*");
            m && (e = Object.keys(t.props));
            let c = e.filter(u => {
                let p = t.props[u];
                if (!p) return !1;
                let {
                    values: E,
                    highlightData: f,
                    type: T,
                    range: d
                } = p, g = [...E];
                if (!g.length) return !1;
                if (o === "range") {
                    let [C] = g, [v, S] = a, b = re(C, v, S, T);
                    return b && s && (f == null || f.set(C, {
                        highlightCSSClass: i
                    })), b
                }
                let A = a.filter(C => {
                    if (d === "from" || d === "to") {
                        let [S, b] = g, x = re(C, S, b, T);
                        return x && s && (f == null || f.set(S, {
                            highlightCSSClass: i
                        }), f == null || f.set(b, {
                            highlightCSSClass: i
                        })), x
                    }
                    return g.some(S => {
                        let b;
                        if (T === "date" && !m) {
                            let [x, z] = [C, S].map(B => {
                                var N;
                                return (N = Tt(B)) == null ? void 0 : N.getTime()
                            });
                            b = x === z
                        } else l.some(({
                            type: x
                        }) => !["checkbox", "radio", "select-one"].includes(x)) || e.length > 1 ? b = S.toLowerCase().includes(C.toLowerCase()) : b = C.toLowerCase() === S.toLowerCase();
                        return b && s && (f == null || f.set(S, {
                            highlightCSSClass: i,
                            filterValue: C
                        })), b
                    })
                });
                return n === "all" ? A.length === a.length : A.length > 0
            });
            return n === "all" ? c.length === e.length : c.length > 0
        },

   // Modified code Line 884
   Ur = (t, { filterKeys: e, values: r, match: n, mode: o, highlight: s, highlightCSSClass: i, elements: l }) => {
              let a = [...r];
              if (!a.length) return !0;
              let m = e.includes("*");
              m && (e = Object.keys(t.props));
              let c = e.every(u => {
                let p = t.props[u];
                if (!p) return !1;
                let { values: E, highlightData: f, type: T, range: d } = p, g = [...E];
                if (!g.length) return !1;
                if (o === "range") {
                  let [C] = g, [v, S] = a, b = re(C, v, S, T);
                  return b && s && (f == null || f.set(C, { highlightCSSClass: i })), b;
                }
                let A = a.every(C => {
                  if (d === "from" || d === "to") {
                    let [S, b] = g, x = re(C, S, b, T);
                    return x && s && (f == null || f.set(S, { highlightCSSClass: i }), f == null || f.set(b, { highlightCSSClass: i })), x;
                  }
                  return g.some(S => {
                    let b;
                    if (T === "date" && !m) {
                      let [x, z] = [C, S].map(B => { var N; return (N = Tt(B)) == null ? void 0 : N.getTime() });
                      b = x === z;
                    } else {
                      l.some(({ type: x }) => !["checkbox", "radio", "select-one"].includes(x)) || e.length > 1
                        ? (b = S.toLowerCase().includes(C.toLowerCase()))
                        : (b = C.toLowerCase() === S.toLowerCase());
                    }
                    return b && s && (f == null || f.set(S, { highlightCSSClass: i, filterValue: C })), b;
                  });
                });
                return A;
              });
              return c;
            },
   ```

   The `filter` method has been changed to `every` to ensure that an item must match all the filter criteria to be included in the results.

2. Inside the `every` loop, the `filter` method for the selected values has also been changed to `every`:

   ```javascript
   // Original code
   let A = a.filter(C => {
     // ...
   });

   // Modified code
   let A = a.every(C => {
     // ...
   });
   ```

   This modification checks if all the selected values are included in the item's values for each filter key.

3. The conditions for returning the final result have been simplified:

   ```javascript
   // Original code
   return n === "all" ? A.length === a.length : A.length > 0;

   // Modified code
   return A;
   ```

   If all the filter keys match (`c` is true), the item is considered valid.

These modifications ensure that the CMS Filter uses an "AND" condition, requiring an item to match all the selected filter criteria to be included in the results.

## Usage

To use the modified CMS Filter attribute in your Webflow project:

1. Copy the updated JavaScript code from the `cmsfilter.js` file in this repository.
2. Replace the original CMS Filter JavaScript code in your Webflow project with the updated code.
3. Ensure that the `re` function is defined before the `Ur` function in the code.
4. Save and publish your Webflow project.

The CMS Filter should now work with the "AND" functionality, showing only the items that match all the selected filter criteria simultaneously.

## License

This modified version of the CMS Filter attribute is released under the [MIT License](https://opensource.org/licenses/MIT). Please refer to the original [Finsweet Attributes repository](https://github.com/finsweet/attributes) for more information on the license and terms of use.
