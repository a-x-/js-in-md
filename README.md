# js-in-md
Plugins for linting and processing js snippets in markdown files

WIP

## prototype
```bash
for f in common.blocks/*/*.ru.md; do
  remark --tree-out $f 2> /dev/null \
  | jq '[.children[] | select(.type? == "code" and .lang? == "js") | .value]' \
  | node -p "
    '['
    +JSON.parse(require('fs').readFileSync('/dev/stdin').toString())
      .map(c=>{var c_=c.trim(); c_.startsWith('{') && (c_='('+c_+')'); return 'function(){\\n'+c_+'\\n}'})
      .join(',\\n')
    +']'" \
  | node -c \
  || echo Error in $f;
done
```
