## API cheat sheet

| Item         | Value                                |
| ------------ | ------------------------------------ |
| Url          | `https://gmgard.com/Ranking/History` |
| **Method**   | `POST`                               |
| **Payload**  | `year=2014&month=1`                  |
| **Required** | `cookie`                             |

Online API develop request via: [Hoppscotch](https://hoppscotch.io/)

## NOTE

1. `GET` method will be return the current month's ranking, there's no difference using payload or not.
2. If not found `Fetch/XHR` in developer tools, type `sessionStorage` in console[^4]. And you will get `storage` info. They stored in session.

```javascript
/** via: https://gmgard.com/Scripts/site.min.js
 * Dame method name! (a, I, K, _, ...)
 */
function _(e, t) {
  return "~MonthRanking" + e + "/" + t;
}
function E() {
  A(),
    y.html(m.getMonthName()),
    w.html(m.getYear()),
    $(".monthly-row").empty().off("click"),
    $(".weekly-body").empty().attr("class", "weekly-body"),
    $("#loading").show(),
    l && !l.status && l.abort();
  var t = m.getYear(),
    n = m.getMonth(),
    e = (function (e, t) {
      if (e == r.getYear() && t == r.getMonth() + 1) return null;
      var n = sessionStorage[_(e, t)];
      return n ? JSON.parse(n) : null;
    })(t, n);
  if (e) return $("#loading").hide(), z(e);
  l = $.post(
    p.data("update-url"),
    {
      year: t,
      month: n,
    },
    function (e) {
      (sessionStorage[_(t, n)] = JSON.stringify(e)), z(e);
    }
  ).always(function () {
    $("#loading").hide();
  });
}
```

3. Vscode could format `json` string directly, after setting `editor.formatOnSave`[^1]. Need regex like `^"` / `"$` / `\"` no more. (And I really reopen and save again each file[^2], just like an idiot.)
4. I discovered this API though the error[^3], which I cannot get the session before `201509`, after I've get a `89` length session storage. (in 202301):

```
Uncaught DOMException: Failed to set a named property on 'Storage': Setting the value of '~MonthRanking2015/8' exceeded the quota.
    at Object.success (https://gmgard.com/Scripts/site.min.js?v=U7hr5ADMwzdYRh2bhBBBp2xZObm9lg1QozGq7ul86a4:1:14598)
    at i (https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.4/jquery.min.js:2:27151)
    at Object.fireWith [as resolveWith] (https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.4/jquery.min.js:2:27914)
    at z (https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.4/jquery.min.js:4:12059)
    at XMLHttpRequest.<anonymous> (https://cdnjs.cloudflare.com/ajax/libs/jquery/2.2.4/jquery.min.js:4:15619)
```

[^1]: via: [formatting - How do I format all files in a Visual Studio Code project? - Stack Overflow](https://stackoverflow.com/questions/43666270/how-do-i-format-all-files-in-a-visual-studio-code-project);
[^2]: navigate between different files via: [Code Navigation in Visual Studio Code](https://code.visualstudio.com/docs/editor/editingevolved); Regex limit via: [Search Editor still has 10000 line limit when search.maxResults is set to a value larger than 10000 · Issue #129153 · microsoft/vscode](https://github.com/microsoft/vscode/issues/129153)
[^3]: via: [javascript - Setting the value of 'dataURL' exceeded the quota - Stack Overflow](https://stackoverflow.com/questions/23977690/setting-the-value-of-dataurl-exceeded-the-quota);
[^4]: via: [.net - How to access Session variables and set them in javascript? - Stack Overflow](https://stackoverflow.com/questions/15519454/how-to-access-session-variables-and-set-them-in-javascript);
