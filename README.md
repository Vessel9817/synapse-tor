# synapse-tor

[![CI](https://github.com/Vessel9817/synapse-tor/actions/workflows/ci.yml/badge.svg)](https://github.com/Vessel9817/synapse-tor/actions/workflows/ci.yml)

## Basic setup

1. Clone the repository and initialize its submodules:

    ```shell
    git clone --recursive https://github.com/Vessel9817/synapse-tor
    ```

1. Enter the top-level project directory:

    ```shell
    cd './synapse-tor'
    ```

<!-- TODO -->

1. Configure secrets:

    ```shell
    cp './grafana/secrets/admin_user.txt.example' './grafana/secrets/admin_user.txt'
    cp './grafana/secrets/admin_pass.txt.example' './grafana/secrets/admin_pass.txt'
    cp './grafana/secrets/admin_email.txt.example' './grafana/secrets/admin_email.txt'
    cp './postgres/config/db.txt.example' './postgres/config/db.txt'
    cp './postgres/config/user.txt.example' './postgres/config/user.txt'
    cp './postgres/config/password.txt.example' './postgres/config/password.txt'
    ```

1. Run the service:

    ```shell
    docker compose up -d grafana tor
    ```

1. Stop the service:

    ```shell
    docker compose down
    ```

[!TIP]

> Shutting down the Postgres service in other ways may result in the service
> being interrupted. This may cause startup time to take significantly longer,
> which may cause health checks to fail until it's finished.

## Running without tor

This project can be run without tor, however, you'll need to expose the website
to the internet through your own means. You'll also need to go through
the configuration steps with your domain, rather than an `.onion` address.

1. Run the service:

    ```shell
    docker compose --profile production up -d nginx-debug
    ```

1. Stop the service:

    ```shell
    docker compose --profile production down
    ```

See the `nginx-debug` container for an example of how to forward traffic
from your machine to the service. You can change the `ports` section to use
a fixed host port, following Docker's port mapping syntax:
`<HOST_PORT>:<CONTAINER_PORT>` or `<HOST_IP>:<HOST_PORT>:<CONTAINER_PORT>`.

[!WARNING]

> In case you accidentally expose your Grafana endpoint over the internet,
> make sure your login credentials are secure.

## Running over tor

There are some issues that should be noted when running this service over tor:

- [Service workers][service workers] are required to use
  [authenticated media][authenticated media]. Tor browser disables this feature
  by default.
- [WebRTC][webrtc] is required to use voice and video calls. Tor browser
  disables this feature by default, but it may be considered if
  [UDP support][udp] is implemented in the tor network. It's been
  [over 20 years][udp-2006], I wouldn't wait around if I were you.

> [!CAUTION]
> *Do not attempt unless you know exactly what you're doing.*

Users can enable some of these features in `about:config` by reversing
some of the steps detailed in
[The Design and Implementation of the Tor Browser][tor-spec],
at the risk of compromising privacy and security.

## Implementation

- [x] [Synapse][synapse]
- [x] [Federation][federation]
- [x] [Metrics][prometheus]
- [x] [Metrics UI (Grafana)][grafana]
- [ ] [Metrics UI (Prometheus)][consoles]
- [ ] [TURN server][turn server]
- [ ] [Workers][workers]

## Licensing

This project follows the same dual-licensing scheme as [Synapse][licenses]. I.e:

- [APGL-3.0](./LICENSE-AGPL-3.0); or
- [Commercial](./LICENSE-COMMERCIAL)

[service workers]: https://github.com/element-hq/element-web/blob/v1.12.12/apps/web/src/serviceworker/index.ts
[authenticated media]: https://github.com/element-hq/element-web/issues/28370
[webrtc]: https://forum.torproject.org/t/safe-way-to-user-webrtc/2231
[udp]: https://spec.torproject.org/proposals/339-udp-over-tor.html
[udp-2006]: https://spec.torproject.org/proposals/100-tor-spec-udp.html
[tor-spec]: https://2019.www.torproject.org/projects/torbrowser/design/

[synapse]: https://blog.facha.dev/how-to-self-host-matrix-and-element-docker-compose/
[federation]: https://element-hq.github.io/synapse/latest/reverse_proxy.html
[prometheus]: https://element-hq.github.io/synapse/latest/metrics-howto.html
[grafana]: https://github.com/element-hq/synapse/blob/master/contrib/grafana
[consoles]: https://github.com/element-hq/synapse/blob/master/contrib/prometheus/README.md
[turn server]: https://element-hq.github.io/synapse/latest/turn-howto.html
[workers]: https://github.com/element-hq/synapse/blob/master/contrib/docker_compose_workers/README.md
[licenses]: [https://github.com/element-hq/synapse/?tab=readme-ov-file#copyright-and-licensing]
