---
title: mocha and chai
created: '2019-03-20T11:19:36.114Z'
modified: '2019-03-20T11:25:33.265Z'
tags: [fundamentals]
---

# mocha and chai

describe("alternatingCase", () => {
  it("if an empty string it should return an empty string", () => {
    expect(alternatingCase("")).to.eql("");
  });

  {expect} = require('chai')
