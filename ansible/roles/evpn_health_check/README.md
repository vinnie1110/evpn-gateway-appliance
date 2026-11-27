# evpn_health_check

Read-only validation suite for the EVPN fabric: BGP session state, EVPN
prefix counts, VXLAN interface presence/absence, and end-to-end ping checks.
Tasks are conditional on inventory group membership, so it's safe to apply
to `hosts: all` (see `playbooks/health-check.yml`).

See [`meta/argument_specs.yml`](meta/argument_specs.yml) for the full input
schema (`stretches`, `tunnel`).

No infrastructure changes are made by this role.

## Workload peers

Infra relay checks iterate the `cloud_workload` inventory group: BGP
Established and EVPN prefixes are required for every peer. On-prem, the
remote VTEPs must be present per VNI. Workload-side checks (against the
standalone VTEP) live in the test collection
(`network.evpn_gateway_test.healthcheck`, on a populated cloud_workload group), since
the production workload side is OCP-native and not SSH-reachable.
