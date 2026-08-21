# デプロイ

`app/` の中身を `gh-pages` ブランチに載せて GitHub Pages で配信する。

```bash
cd C:\dev\bowling-tally
git add -A && git commit -m "..."
git push
git subtree push --prefix app origin gh-pages
```

公開 URL: https://chan1018.github.io/bowling-tally/

> GitHub Actions を使わないのは、この PC の `gh` トークンに `workflow` スコープが無く、
> `.github/workflows/` を push できないため。`gh auth refresh -s workflow` を通せば
> Actions 方式に切り替えられるが、ブラウザ再認証が要る。

**中身を更新したら `git subtree push` を忘れないこと。** main に push しただけでは公開版は変わらない。
Service Worker が cache-first なので、端末側は次回起動時に反映される（`app/sw.js` の `CACHE` の版を上げると確実）。
