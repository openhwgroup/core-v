**March 14th, 2024**

**Prepared by: Cesar Fuguet**

# CV-HPDCACHE

## Summary

Development and verification of a High-Performance L1 Dcache integrated with
the CVA6.

## Activities and progress since last report:

* Achievements:
  * Functional integration with OpenPiton running Linux with up to 64 cores !
  * Several features implemented in the HPDcache to support tiny embedded
    configurations: single entry MSHR, pseudo random replacement policy
    (using a 8-bit LFSR).
  * Thales DIS is doing a parameter exploration of the HPDcache to target
    tiny embedded systems. They achieved better area results than with the
    WTDcache. Therefore, they are switching to the HPDcache for future
    products (together with the CVA6 core).
  * First complete version of the HPDcache User Guide in ReadTheDocs format
    (restructured text). Not yet published in the OpenHW ReadTheDocs server.

* Planned:
  * Still working in the delivery of a preliminary testplan. We targeted
    end of march but we will probably have a 4 or 5 weeks delay due to
    internal staff planning.
  * Working in the delivery of a preliminary testbench
  * J. Balkind, N. Oliete, and myself are proposing a Google Summer of Code
    projet to integrate the HPDcache also as an instruction cache. Target
    cores: CVA6, Sargantana, and/or Lagarto cores.

* Unplanned:
  * NA

## Issues and dificulties:

  * Staff

## Gate status:

  * Plan Approved

## Schedule:

  * Delivery of a up-to-date user guide end of March'24
  * Delivery of a preliminary test-plan end of March'24 (delayed to April-May'24)
  * Delivery of a preliminary UVM test-bench end of April'24 (delayed to May'24)

