## Sample Bid Requests
1. [Native Bid Request](#native-bid-request)
2. [Native 1.2 Bid Request](#native-12-bid-request)
3. [Wizard Bid Request](#wizard-bid-request)
4. [Video Bid Request](#video-bid-request)
5. [Header Direct Bid Request](#header-direct-bid-request)
### Native Bid Request
```json
{
  "id": "161991970754563428110",
  "at": 2,
  "tmax": 178,
  "imp": [
    {
      "id": "1",
      "tagid": "21979",
      "pmp": {
        "private_auction": 0,
        "deals": [

        ]
      },
      "ext": {
        "wopv": "dd855c93-1f53-11e9-b40d-02753142902a"
      },
      "native": {
        "ver": 1,
        "request": "{\"ver\":1,\"layout\":6,\"adunit\":5,\"plcmtcnt\":1,\"seq\":0,\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len\":100}},{\"id\":2,\"required\":1,\"img\":{\"type\":3,\"w\":300,\"wmin\":225,\"h\":150,\"hmin\":113}},{\"id\":3,\"required\":0,\"img\":{\"type\":2,\"wmin\":25,\"hmin\":25}},{\"id\":4,\"required\":0,\"data\":{\"type\":2,\"len\":300}},{\"id\":5,\"required\":1,\"data\":{\"type\":1,\"len\":30}},{\"id\":6,\"required\":0,\"video\":{\"mimes\":[\"video/mp4\",\"video/webm\",\"video/mpeg\",\"application/javascript\"],\"minduration\":1,\"maxduration\":900,\"protocols\":[1,2,3,4,5,6,7,8],\"ext\":{\"playbackmethod\":[3,2]}}}]}",
        "ext": {
          "triplelift": {
            "formats": [
              1,
              2,
              3,
              4,
              5,
              6,
              7,
              8,
              9,
              10
            ]
          }
        },
        "api": [
          1,
          2
        ]
      }
    }
  ],
  "site": {
    "id": "1791911",
    "domain": "trend-chaser.com",
    "cat": [
      "IAB8",
      "IAB24",
      "210"
    ],
    "page": "http://www.trend-chaser.com/entertainment/movies/the-true-story-behind-the-blind-side/3/",
    "publisher": {
      "id": "6263",
      "cat": [
        "IAB8",
        "IAB24",
        "210"
      ]
    }
  },
  "device": {
    "ua": "Mozilla/5.0 (iPhone; CPU iPhone OS 12_1_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16C101 [Pinterest/iOS]",
    "geo": {
      "type": 2,
      "country": "USA",
      "region": "DC",
      "metro": "511",
      "city": "Washington",
      "zip": "20036"
    },
    "ip": "99.203.17.245",
    "devicetype": 4,
    "make": "Apple",
    "model": "iPhone",
    "os": "iOS",
    "osv": "12",
    "language": "en"
  },
  "user": {},
  "regs": {
    "coppa": 0
  },
  "source": {
    "pchain": "6c33edb13117fd86:4887"
  }
}
```
### Native 1.2 Bid Request
```json
{
  "id": "65673520512019368150",
  "at": 2,
  "tmax": 184,
  "imp": [
    {
      "id": "1",
      "tagid": "22793",
      "secure": 1,
      "ext": {
        "wopv": "25ab6152-ed16-4e1b-ba9d-d80f353544f8"
      },
      "native": {
        "ver": "1.2",
        "request": "{\"ver\":\"1.2\",\"context\":1,\"plcmttype\":1,\"plcmtcnt\":1,\"seq\":0,\"aurlsupport\":1,\"privacy\":1,\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len\":100}},{\"id\":2,\"required\":1,\"img\":{\"type\":3,\"w\":320,\"wmin\":240,\"h\":50,\"hmin\":38}},{\"id\":3,\"required\":0,\"img\":{\"type\":2,\"wmin\":25,\"hmin\":25}},{\"id\":4,\"required\":0,\"data\":{\"type\":2,\"len\":300}},{\"id\":5,\"required\":1,\"data\":{\"type\":1,\"len\":30}},{\"id\":6,\"required\":0,\"video\":{\"mimes\":[\"video/mp4\",\"video/webm\",\"video/mpeg\",\"application/javascript\"],\"minduration\":1,\"maxduration\":900,\"protocols\":[1,2,3,4,5,6,7,8],\"ext\":{\"playbackmethod\":[3]}}}],\"eventtrackers\":[{\"event\":1,\"methods\":[1,2]},{\"event\":2,\"methods\":[1]}]}",
        "ext": {
          "triplelift": {
            "formats": [
              1,
              2,
              4,
              5,
              6,
              9,
              10
            ]
          }
        },
        "api": [
          1,
          2
        ]
      }
    }
  ],
  "site": {
    "id": "37059",
    "domain": "littlethings.com",
    "cat": [
      "IAB18",
      "IAB9",
      "IAB24",
      "IAB1",
      "IAB8",
      "IAB7",
      "IAB10",
      "IAB12",
      "579",
      "560",
      "239",
      "42",
      "210",
      "223",
      "274",
      "379"
    ],
    "page": "https://www.littlethings.com/student-dies-leftover-pasta?utm_source=LTcom&utm_medium=Facebook&utm_campaign=celebrity&utm_content=1NC",
    "publisher": {
      "id": "5921",
      "cat": [
        "IAB18",
        "IAB9",
        "IAB24",
        "IAB1",
        "IAB8",
        "IAB7",
        "IAB10",
        "IAB12",
        "579",
        "560",
        "239",
        "42",
        "210",
        "223",
        "274",
        "379"
      ]
    }
  },
  "device": {
    "ua": "Mozilla/5.0 (Linux; Android 6.0.1; SM-S327VL Build/MMB29M; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/74.0.3729.157 Mobile Safari/537.36 [FB_IAB/FB4A;FBAV/225.0.0.47.118;]",
    "geo": {
      "type": 2,
      "country": "USA",
      "region": "FL",
      "metro": "686",
      "city": "Baker",
      "zip": "32531"
    },
    "ip": "173.31.128.159",
    "devicetype": 4,
    "make": "Google",
    "os": "Android",
    "osv": "6",
    "language": "en"
  },
  "user": {
    "id": "16515134754534630770",
    "buyeruid": "U8G2KgeasyVLwuYpUMX6JQCS5ixLwLUrVsYHpc66"
  },
  "source": {
    "pchain": "6c33edb13117fd86:2992"
  }
}
```
### Wizard Bid Request
```json
{
  "id": "135270485388854379200",
  "at": 2,
  "tmax": 949,
  "imp": [
    {
      "id": "1",
      "tagid": "18419",
      "secure": 1,
      "banner": {
        "w": 3,
        "h": 3,
        "ext": {
          "triplelift": {
            "formats": [
              1,
              2,
              3,
              4,
              5,
              6,
              10
            ]
          }
        }
      },
      "pmp": {
        "private_auction": 0,
        "deals": [

        ]
      },
      "ext": {
        "wopv": "22c5513c-1f54-11e9-90df-024c73a9dfe6"
      }
    }
  ],
  "site": {
    "id": "1951283",
    "domain": "modernhoney.com",
    "cat": [
      "IAB9",
      "IAB24",
      "IAB8",
      "IAB7",
      "239",
      "210",
      "223"
    ],
    "page": "https://www.modernhoney.com/back-to-school-kids-lunch-ideas/",
    "publisher": {
      "id": "4116",
      "cat": [
        "IAB9",
        "IAB24",
        "IAB8",
        "IAB7",
        "239",
        "210",
        "223"
      ]
    }
  },
  "device": {
    "ua": "Mozilla/5.0 (iPhone; CPU iPhone OS 12_1_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Mobile/15E148 Safari/604.1",
    "geo": {
      "type": 2,
      "country": "GBR",
      "region": "BNE",
      "city": "Hendon",
      "zip": "NW4"
    },
    "ip": "86.173.71.98",
    "devicetype": 4,
    "make": "Apple",
    "model": "iPhone",
    "os": "iOS",
    "osv": "12",
    "language": "en"
  },
  "user": {
    "ext": {
      "consent": "BOXB2pJOXB2pJABABAENAW-AAAAQCABgACAgAA"
    }
  },
  "regs": {
    "ext": {
      "gdpr": 1
    }
  },
  "source": {
    "pchain": "6c33edb13117fd86:3314"
  }
}
```
### Video Bid Request
```json
{
  "id": "125119514492643339213",
  "at": 2,
  "tmax": 949,
  "imp": [
    {
      "id": "1",
      "tagid": "21600",
      "secure": 1,
      "video": {
        "mimes": [
          "video/mp4",
          "video/webm",
          "video/mpeg",
          "application/javascript"
        ],
        "minduration": 1,
        "maxduration": 900,
        "protocols": [
          1,
          2,
          3,
          4,
          5,
          6,
          7,
          8
        ],
        "w": 500,
        "h": 281,
        "placement": 3,
        "boxingallowed": 1,
        "playbackmethod": [
          2
        ],
        "api": [
          1,
          2
        ]
      },
      "pmp": {
        "private_auction": 0,
        "deals": [

        ]
      },
      "ext": {
        "wopv": "c96e7d43-1f53-11e9-b40d-02753142902a"
      }
    }
  ],
  "site": {
    "id": "38957",
    "domain": "baseball-reference.com",
    "cat": [
      "IAB14-7",
      "IAB18",
      "IAB9",
      "IAB19",
      "IAB24",
      "212"
    ],
    "page": "https://www.baseball-reference.com/players/e/eisenji01.shtml",
    "publisher": {
      "id": "5579",
      "cat": [
        "IAB14-7",
        "IAB18",
        "IAB9",
        "IAB19",
        "IAB24",
        "212"
      ]
    }
  },
  "device": {
    "ua": "Mozilla/5.0 (iPad; CPU OS 12_1_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Mobile/15E148 Safari/604.1",
    "geo": {
      "type": 2,
      "country": "USA",
      "region": "MI",
      "metro": "505",
      "city": "Ypsilanti",
      "zip": "48198"
    },
    "ip": "198.111.167.210",
    "devicetype": 5,
    "make": "Apple",
    "model": "iPad",
    "os": "iOS",
    "osv": "12",
    "language": "en"
  },
  "user": {},
  "source": {
    "pchain": "6c33edb13117fd86:4796"
  }
}
```
### Header Direct Bid Request
```json
{
  "id": "71138340581185099350",
  "at": 1,
  "tmax": 148,
  "imp": [
    {
      "id": "1",
      "tagid": "hd_25329",
      "secure": 1,
      "banner": {
        "w": 300,
        "h": 600
      },
      "bidfloor": 0,
      "ext": {
        "wopv": "b888f895-1f42-11e9-aeb2-027cafddf576"
      }
    }
  ],
  "site": {
    "id": "37884",
    "domain": "gocomics.com",
    "cat": [],
    "page": "https://www.gocomics.com/thebarn/2019/01/23",
    "publisher": {
      "id": "1812",
      "cat": []
    }
  },
  "device": {
    "ua": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_1) AppleWebKit/604.3.5 (KHTML, like Gecko) Version/11.0.1 Safari/604.3.5",
    "geo": {
      "type": 2,
      "country": "CAN",
      "region": "SK",
      "city": "Weyburn",
      "zip": "S4H"
    },
    "ip": "24.72.100.200",
    "devicetype": 2,
    "make": "Apple",
    "os": "Mac OS X",
    "language": "en"
  },
  "user": {
    "id": "18364214429884349062"
  },
  "source": {
    "pchain": "6c33edb13117fd86:1824"
  }
}
```

