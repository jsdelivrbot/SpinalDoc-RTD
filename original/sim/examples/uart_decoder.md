---
type: homepage
title: "SpinalSim UART decoder example"
tags: [user guide]
keywords: spinal, user guide
last_updated: Apr 19, 2016
sidebar: spinal_sidebar
toc: true
permalink: /spinal/sim/example/uart_decoder/
---


```scala
//Fork a simulation process which will analyse the uartPin and print transmited bytes into the simulation terminal
fork{
  //Wait until the design put the uartPin to true (wait the reset effect)
  waitUntil(uartPin.toBoolean == true)

  while(true) {
    waitUntil(uartPin.toBoolean == false)
    sleep(baudPeriod/2)

    assert(uartPin.toBoolean == false)
    sleep(baudPeriod)

    var buffer = 0
    (0 to 7).suspendable.foreach{ bitId =>
      if(uartPin.toBoolean)
        buffer |= 1 << bitId
      sleep(baudPeriod)
    }

    assert(uartPin.toBoolean == true)
    print(buffer.toChar)
  }
}
```
