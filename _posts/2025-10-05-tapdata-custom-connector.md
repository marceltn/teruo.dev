---
layout: post
title: “When Open Source Saved My Data Pipeline”
date: 2025-10-05 08:00:00 +0900
tags: [opensource, data, engineering, mariadb]
categories:
  - tapdata
  - tapdata-connector
  - engineering
---

# When Open Source Saved My Data Pipeline

## Context & Motivation
I ran into a real production issue while working with MariaDB 11.4 and TapData: **CDC (Change Data Capture) was simply not working for MariaDB versions greater than 8** in my scenario. This wasn’t a tuning problem or a missing flag. After digging into it, the limitation was clearly inside the connector’s implementation.

At that point, the realistic options were limited:

- Contact the maintainers and wait for their support
- Downgrade the database (not acceptable)
- Or take ownership and fix the problem myself

Because the system was **open source**, the third option was actually possible.

You *can* build a custom connector entirely from scratch. In my case, I chose a more pragmatic and lower-risk approach: I duplicated the existing MariaDB connector and modified only the parts that mattered for my CDC use case.

---

## Open Source: Power *and* Responsibility

It’s easy to romanticize open source, but the reality is more nuanced.

Yes, having access to the code meant I could:
- Inspect the existing MariaDB connector
- Understand how CDC was implemented internally
- Confirm that the limitation wasn’t something I could configure away

But it also meant:
- There was no guaranteed support SLA
- Documentation didn’t cover this edge case
- I had to learn the system deeply on my own and can take more time than waiting for help or choose another tool.

Open source gives you freedom — but it also means **you are often your own support line**. In this situation, that trade-off made sense.

---

## Duplicating the MariaDB Connector (Carefully)

Rather than rebuilding everything from zero, I duplicated the existing `mariadb-connector` module and adapted it.

To avoid conflicts with the built-in connector, a few things were critical:

- **artifactId** had to be changed so the new connector could coexist safely  
- **Spec file** located at:

```bash
./tapdata-connectors/connectors/mariadb-connector/src/main/resources/spec_mariadb.json
```

was duplicated and renamed, with all internal IDs and names updated so it registered as a *new* connector.

- Only the CDC-related logic was modified; the rest of the implementation remained untouched

This approach minimized risk while still giving me full control where the default behavior fell short.

---

## Building and Registering the Connector

After building the connector into a JAR, I registered it using the PDK:

```bash
java -jar /build/pdk register \
  -a ${access_code} \
  -ak ${access-key} \
  -sk ${secret-key} \
  -r ${oem-type} \
  -f ${need-register-connector-type} \
  -t http://${tm-server} \
  /connectors/dist/demo-mariadb-connector.jar
```

It fails...

---

## Version Compatibility: A Hard-Learned Lesson

Not because the logic was wrong — but because **the PDK version I used to register the connector did not match the version running on the server**.

This wasn’t obvious at first. The error messages were vague, and nothing clearly pointed to a version mismatch.

What finally helped was inspecting the **MANIFEST file inside the PDK JAR** that came with the server distribution. That file contained the exact version information I needed.

Once I aligned everything, the problem disappeared.

The working combination was:

- TapData server version: **v3.27.0**
- PDK / connectors version: **release-v1.4.4**

```bash
git clone https://github.com/tapdata/tapdata.git
cd tapdata
git checkout release-v3.27.0

git clone https://github.com/tapdata/tapdata-connectors.git
cd tapdata-connectors
git checkout release-v1.4.4
```

This was a reminder that, when working close to infrastructure, **version mismatches can look like logic bugs**.

Once the PDK version matched the server version, registration succeeded and the connector appeared as a first-class option in the UI.

---

## Testing, Iteration, and Confidence

With the custom connector in place, the focus shifted to validation:

- Schema discovery  
- Full sync behavior  
- CDC correctness on MariaDB > 8  
- Failure and retry scenarios  
- Performance under realistic load  

Each iteration reduced uncertainty and increased confidence that the pipeline would behave correctly in production.

---

## The Real Takeaway

This experience wasn’t about open source ideology.

It was about **having an escape hatch when the default path is blocked**.

In a closed system, my only option would have been to wait — and hope. In this case, waiting wasn’t acceptable.

Open source didn’t make the problem easy. It required time, reading code, understanding internals, and accepting limited external support. But it gave me the ability to move forward when no workaround existed.

That ability matters when correctness and reliability are non-negotiable.

---

Open source didn’t remove friction — but it gave me control when it mattered most.

---

