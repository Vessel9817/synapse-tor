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
  docker compose --profile production up -d
  ```

## Running without tor

By removing the `tor`, `tor-init` and `vanguards` containers from
[`docker-compose.yml`](./docker-compose.yml)

## Running over tor

There are some issues that should be noted when running this service over tor:

- [Service workers][service workers] are required to use
  [authenticated media][authenticated media]. Tor browser disables this feature
  by default.
- [WebRTC][webrtc] is required to use voice and video calls. Tor browser
  disables this feature by default, but it may be considered if
  [UDP support][udp] is implemented in the tor network. It's been
  [over 20 years][udp-2006], I wouldn't wait around if I were you.

Users can enable these features in `about:config`, at the risk of reducing
their security and privacy.
*Not recommended unless you know exactly what you're doing.*

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

[synapse]: https://blog.facha.dev/how-to-self-host-matrix-and-element-docker-compose/
[federation]: https://element-hq.github.io/synapse/latest/reverse_proxy.html
[prometheus]: https://element-hq.github.io/synapse/latest/metrics-howto.html
[grafana]: https://github.com/element-hq/synapse/blob/master/contrib/grafana
[consoles]: https://github.com/element-hq/synapse/blob/master/contrib/prometheus/README.md
[turn server]: https://element-hq.github.io/synapse/latest/turn-howto.html
[workers]: https://github.com/element-hq/synapse/blob/master/contrib/docker_compose_workers/README.md
[licenses]: [https://github.com/element-hq/synapse/?tab=readme-ov-file#copyright-and-licensing]
