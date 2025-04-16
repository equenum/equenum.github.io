---
title: Load testing a self-hosted PostgreSQL instance
date: 2024-09-22 13:00:00 +0400
categories: [homelab, benchmarks]
tags: [benchmark, postgres, server, self-hosting]
image:
  path: assets/img/posts/20240922/postgre-load-thumb.webp
  lqip: assets/img/posts/common/common-thumb-lqip.webp
---

As someone who runs their homelab on older, low-power hardware, I constantly wonder if the setup I have is up to the task and can handle some semi-decent workloads if I ever need to expand the circle of users beyond just friends and family. There is probably no better way to answer this question than by actually trying.

Luckily, I recently got a convenient chance to do exactly that, at least to some degree. While working on a [blog about testing UUID primary key performance]({% post_url 2024-07-22-uuid-v4-vs-v7-in-postgre-sql %}), I happened to run hundreds of thousands of write requests against my PostgreSQL instance. As a pleasant side effect of that, I found myself with some interesting metrics I was able to collect via the built-in VM resource metrics dashboard in Proxmox.

Here, I want to document and share those metrics. A quick disclaimer before we begin: the test method I used is probably quite far from how proper load/performance testing is conducted and might not accurately represent real-life use cases. I am just sharing my journey while trying to learn something new.

## Homelab setup

### Hardware

For this experiment, I used my trusted HP EliteDesk 800 G3 Mini, a 1-liter computer. Quite low spec, but fairly power efficient (35 W). Feel free to read more about it in a [dedicated blog entry]({% post_url 2024-05-06-my-homelab-setup-update-v1.1 %}).

Here is a quick hardware overview:

- Intel i5-6500T: 4 cores, 4 threads, 2.5/3.1 (base/turbo) GHz frequency.
- 24 GB DDR4-2400 RAM.
- 2.5” SSD Patriot Burst PBE240GS25SSDR (450 / 320, Seq. Read / Write, MB/s).

### Virtualization

I hosted my deployment in a Linux virtual machine:

- Virtualization platform: [Proxmox Virtual Environment](https://www.proxmox.com/en/) 8.2.5.
- Host OS: Debian “Bookworm” 12.7 (headless).
- RAM: 8 GiB.
- Processors: 2 (1 socket, 2 cores).
- Hard Disk: 32 GB.

### Deployment

Here goes the description of my PostgreSQL instance deployment:

- PostgreSQL 15.8, native install.
- Docker Engine + 2 containers: [pgAdmin4](https://www.pgadmin.org/download/pgadmin-4-container/), [Portainer](https://www.portainer.io/).

All other VMs were temporary stopped for the length of the test. There were also no database traffic other from that produced by the experiment.

## Approach

My test approach is described in more detail in the [dedicated blog entry]({% post_url 2024-07-22-uuid-v4-vs-v7-in-postgre-sql %}) mentioned previously, but long story short, I was running a simple `.NET 8` console application that would run write queries against the database via `Entity Framework Core`. The app was deployed as `Docker` containers on my laptop to provide an easy way to simulate separate individual simultaneous clients.

Metrics collected:

- CPU usage.
- Memory usage.
- Disk IO.
- Network traffic.

Before, we proceed with the tests, here is how the VM baseline resource consumption looked like:

**CPU usage:**

![Desktop View](assets/img/posts/20240922/baseline/cpu.png){: width="730" height="330" }

**Memory usage:**

![Desktop View](assets/img/posts/20240922/baseline/ram.png){: width="730" height="330" }

**Disk IO:**

![Desktop View](assets/img/posts/20240922/baseline/disk.png){: width="730" height="330" }

**Network traffic:**

![Desktop View](assets/img/posts/20240922/baseline/network.png){: width="730" height="330" }

## Results

### 1 client 80k writes

**CPU usage:**

![Desktop View](assets/img/posts/20240922/1_client/cpu.png){: width="730" height="330" }

**Memory usage:**

![Desktop View](assets/img/posts/20240922/1_client/ram.png){: width="730" height="330" }

**Disk IO:**

![Desktop View](assets/img/posts/20240922/1_client/disk.png){: width="730" height="330" }

**Network traffic:**

![Desktop View](assets/img/posts/20240922/1_client/network.png){: width="730" height="330" }

### 1 client, 3 consecutive runs (22k writes each)

**CPU usage:**

![Desktop View](assets/img/posts/20240922/1_client_3_runs/cpu.png){: width="730" height="330" }

**Memory usage:**

![Desktop View](assets/img/posts/20240922/1_client_3_runs/ram.png){: width="730" height="330" }

**Disk IO:**

![Desktop View](assets/img/posts/20240922/1_client_3_runs/disk.png){: width="730" height="330" }

**Network traffic:**

![Desktop View](assets/img/posts/20240922/1_client_3_runs/network.png){: width="730" height="330" }

### 2 simultaneous clients (22k writes each)

**CPU usage:**

![Desktop View](assets/img/posts/20240922/2_clients/cpu.png){: width="730" height="330" }

**Memory usage:**

![Desktop View](assets/img/posts/20240922/2_clients/ram.png){: width="730" height="330" }

**Disk IO:**

![Desktop View](assets/img/posts/20240922/2_clients/disk.png){: width="730" height="330" }

**Network traffic:**

![Desktop View](assets/img/posts/20240922/2_clients/network.png){: width="730" height="330" }

### 4 simultaneous clients (22k writes each)

**CPU usage:**

![Desktop View](assets/img/posts/20240922/4_clients/cpu.png){: width="730" height="330" }

**Memory usage:**

![Desktop View](assets/img/posts/20240922/4_clients/ram.png){: width="730" height="330" }

**Disk IO:**

![Desktop View](assets/img/posts/20240922/4_clients/disk.png){: width="730" height="330" }

**Network traffic:**

![Desktop View](assets/img/posts/20240922/4_clients/network.png){: width="730" height="330" }

### Average query execution time

Average query execution time was roughly around `5 ms` (taken from the console application logs).

```shell
info: Executed DbCommand (4ms)
info: Executed DbCommand (5ms)
info: Executed DbCommand (7ms)
info: Executed DbCommand (9ms)
info: Executed DbCommand (4ms)
info: Executed DbCommand (4ms)
info: Executed DbCommand (4ms)
info: Executed DbCommand (4ms)
info: Executed DbCommand (4ms)
info: Executed DbCommand (4ms)
info: Executed DbCommand (4ms)
```

## Conclusion

It seems like the VM was able to handle the workload without breaking a sweat, especially considering that at least `260k+` records were written to the database in the span of around 2 hours while demonstrating decent query execution time. This makes me very enthusiastic about my current setup and the viability of messing around with older used hardware in general.

As usual, I hope you found this blog entry useful. Feel free to reach out to me in case of any questions, to share your feedback, findings, or ideas.

## References and further reading

- [Benchmarking UUID v4 vs v7 primary keys in Postgres]({% post_url 2024-07-22-uuid-v4-vs-v7-in-postgre-sql %})
- [My Homelab setup update 2024v1.1]({% post_url 2024-05-06-my-homelab-setup-update-v1.1 %})
- [Proxmox](https://www.proxmox.com/en/)
- [pgAdmin 4 (Container)](https://www.pgadmin.org/download/pgadmin-4-container/)
- [Portainer](https://www.portainer.io/)
