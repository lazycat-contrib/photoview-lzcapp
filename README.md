# Photoview LazyCat community package

Upstream https://github.com/photoview/photoview (AGPL-3.0), 2.4.0. Package `community.lazycat.app.photoview`, min OS 1.6.0 (needed for `run_as`), amd64, mirror delivery, community (喵喵) store only.

Upstream's example deployment pairs the app with PostgreSQL, but the server supports SQLite (`PHOTOVIEW_DATABASE_DRIVER=sqlite`) and the image already sets `PHOTOVIEW_SERVE_UI=1`, so the whole gallery runs as **one container**:

    /lzcapp/var/data:/data      # SQLite database + thumbnail cache
    /lzcapp/var/photos:/photos  # the photo folders you point libraries at

The image creates its account as `photoview` with **uid/gid 999** (verified from the image build history, not assumed), so `run_as: "999:999"` maps the persistent directory owner. `run_as` is mutually exclusive with a service-level `setup_script`, so no chown script is used.

No `deploy_params.yml` and no `public_path`: nothing is pre-seeded, and the first admin is created in the browser on first visit.

Known limits: photos must be placed in `/lzcapp/var/photos` yourself; face recognition models are not bundled; listing and scanning a very large library is untested.

Not device-tested: first admin creation, library scan, thumbnails, timeline browsing, sharing links, and persistence across restarts.
