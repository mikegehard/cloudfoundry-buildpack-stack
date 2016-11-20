# [Cloud Foundry Buildpack for Stack based Haskell projects][1]

[Cloud Foundry buildpack][2] for [Stack][3] based Haskell projects. 
Based on the excellent [heroku-buildpack-stack][4].

## Usage

Push an app with version 1.0 of this buildpack:

    $ cf push haskell-api -b  https://github.com/mikegehard/cloudfoundry-buildpack-haskell-stack#1.0 -m 2GB

**Note: Best to set the memory for the application at 2GB. This makes sure the compilation VM
has enough memory to compile the Haskell. I am looking into ways to eliminate this requirement by setting the compilation container memory independently from the running container memory.**

## App constraints

The application must pull the port to use from the `PORT` environment variable.

An example [Scotty][5] app:

```
main :: IO ()
main = do
    putStrLn "Starting server..."
    port <- lookupEnv "PORT"
    scotty (maybe 3000 read port) $ do
        WebApp.routes
```

See [here][6] for source code of an example app deployed to [PWS][7].

[1]: https://github.com/mikegehard/cloudfoundry-buildpack-haskell-stack
[2]: https://docs.cloudfoundry.org/buildpacks/custom.html
[3]: https://github.com/commercialhaskell/stack
[4]: https://github.com/mfine/heroku-buildpack-stack
[5]: https://github.com/scotty-web/scotty
[6]: https://github.com/mikegehard/haskell-api
[7]: https://run.pivotal.io
