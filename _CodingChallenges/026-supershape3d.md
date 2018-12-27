---
title: "3D Supershapes"
redirect_from: CodingChallenges/26-supershape3d.html
video_number: 26
date: 2016-06-30
video_id: akM4wMZIBWg
repository: CC_026_SuperShape3D
links:
  - title: "Supershapes (Superformula)"
    url: "http://paulbourke.net/geometry/supershape/"
    author: "Paul Bourke"
  - title: "3D Supershapes"
    url: "http://www.syedrezaali.com/3d-supershapes/"
    author: "Reza Ali"
  - title: "Spherical Coordinates System on Wikipedia"
    url: "https://en.wikipedia.org/wiki/Spherical_coordinate_system"
  - title: "Peasycam"
    url: "http://mrfeinberg.com/peasycam/"
    author: Jonathan Feinberg
---

In this coding challenge, I use the "superformula" to make a 3D "supershape" in Processing.  This is part 4 of a multi-part series on superformulas, superellipses and supershapes

Edited by: Morten Haahr Kristensen.
Version: P5 version 1.0


let total = 50;
let globe = [];

let offset = 0;

let m = 0;
let mchange = 0;

let a = 1;
let b = 1;

function setup() {
  createCanvas(600, 600, WEBGL);
  colorMode(HSB);


  for (let i = 0; i < total + 1; i++) {
    globe[i] = [];
    for (let j = 0; j < total + 1; j++) {
      globe[i][j] = createVector(0, 0, 0);
    }
  }
}

function supershape(theta, m, n1, n2, n3) {
  let t1 = abs((1 / a) * cos(m * theta / 4));
  t1 = pow(t1, n2);
  let t2 = abs((1 / b) * sin(m * theta / 4));
  t2 = pow(t2, n3);
  let t3 = t1 + t2;
  let r = pow(t3, -1 / n1);

  return r;
}


function draw() {
  background(0);

  camera(500, 500, 700, 0, 0, 0, 0, 1, 0); // Jo hørere 3. værdi, jo mere zoomet ud.
  orbitControl();
  noStroke();


  m = map(sin(mchange), -1, 1, 0, 7);
  mchange += 0.02;
  let r = 200;
  for (let i = 0; i < total + 1; i++) {
    let lat = map(i, 0, total, -HALF_PI, HALF_PI);
    let r2 = supershape(lat, m, 0.2, 1.7, 1.7);
    for (let j = 0; j < total + 1; j++) {
      let lon = map(j, 0, total, -PI, PI);
      let r1 = supershape(lon, m, 0.2, 1.7, 1.7);
      let x = r * r1 * cos(lon) * r2 * cos(lat);
      let y = r * r1 * sin(lon) * r2 * cos(lat);
      let z = r * r2 * sin(lat);
      globe[i][j].set(x, y, z);
    }
  }

  offset += 5;
  for (let i = 0; i < total; i++) {
    let hu = map(i, 0, total, 0, 255 * 6);
    fill(hu % 255, 255, 255);
    beginShape(TRIANGLE_STRIP);
    for (let j = 0; j < total + 1; j++) {
      var v1;
      v1 = globe[i][j];
      vertex(v1.x, v1.y, v1.z);
      var v2;
      v2 = globe[i + 1][j];
      vertex(v2.x, v2.y, v2.z);
    }
    endShape();
  }
}
