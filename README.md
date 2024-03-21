**See https://github.com/vercel/vercel/issues/11318**

---

When Vercel detects a Turborepo (containing `build` pipeline), the Output Directory no longer falls back to `.` if `public` does not exist after build.

I have a minimal reproducible repo which is simply a Turborepo with a single app, which is nothing but a static `index.html` page: https://github.com/yescomment/vc-mre-turbooutputbug/. Deploy it to Vercel with a root directory of `./apps/static-html`.

## No Turbo detected ([example public deployment](https://vercel.com/nocomment/vc-mre-turbooutputbug/ChnyWja2nzadDiBS31Fjb3HWozWF))

When the `build` key is removed from `turbo.json`, the app deploys without error, as the static site has no `public` directory, so falls back to serving `.`:

## Turbo detected ([example public deployment](https://vercel.com/nocomment/vc-mre-turbooutputbug/EXfeXRSaBUX33k8gmS3R5XJautTa))

When even an empty `build` key is added to `turbo.json`, the static app encounters the build error:

```
Error: No Output Directory named "public" found after the Build completed. You can configure the Output Directory in your Project Settings.
```

…even though the Output Directory setting continues to display `` `public` if it exists, or `.` ``
![Screenshot 2024-03-21 at 6 56 51 PM](https://github.com/vercel/vercel/assets/2448782/6d1334ec-6e17-47aa-abb1-3ac5a1dcf14e)

Perhaps this a documented default behavior of Turborepo? However I think the Output Directory placeholder value should update to show that `.` will no longer be automatically used if `public` does not exist.