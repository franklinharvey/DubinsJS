# DubinsJS
A Typescript/Javascript Translation of Andrew Walker's C Implementation of a Dubin's Curves Algorithm

## Recommended Reading (and viewing)
Please also check out [Andrew Walker's repo](https://github.com/AndrewWalker/Dubins-Curves) for the code I based this off of, as well [this python version](https://github.com/AndrewWalker/pydubins)

I also highly recomend checking out [this blog post that](https://gieseanw.wordpress.com/2012/10/21/a-comprehensive-step-by-step-tutorial-to-computing-dubins-paths/) explains and breaks down Dubins's algorithm. It also features a [really fun video](https://www.youtube.com/watch?v=fEImNJQ3hUM) that I think offers an excellent visual explanation

## Example
```
const newPoints: [number, number][] = []
const dubWorker = new Dubins()
dubWorker.shortestAndSample([
    // start x, y, and heading
    start.x,
    start.y,
    start.heading // in radians
  ], [
    // end x, y, and heading
    end.x,
    end.y,
    end.heading // in radians
  ],
  turning_radius,
  step_size, 
  (q, _) => {
    // callback, you will need this in order to get your interpolated points
    newPoints.push([q[0], q[1]])
    return 0
  })
```

#### What if I want to use Lat/Long instead of X/Y?
Short answer, __don't do this__. I do it, but you shouldn't. If you *realllly* want to then you can do this:
```
// all units in degrees
const newPoints: [number, number][] = []
const turning_radius = 0.00001 // ~ 1 meter
const step_size = 0.000003 // ~ .3 meters

// last point of first segment, assuming array of latLng's
const start = firstSegment[firstSegment.length - 1]

// compute heading between second to last and last point
const startHeading = google.maps.geometry.spherical.computeHeading(firstSegment[firstSegment.length - 2], firstSegment[firstSegment.length - 1])

// first point of last segment, assuming array of latLng's
const end = lastSegment[0]
// compute heading between first and second point
const endHeading = google.maps.geometry.spherical.computeHeading(endHeading[0], endHeading[1])

const dubWorker = new Dubins()
dubWorker.shortestAndSample([
    // start x, y, and heading
    start.lat(),
    start.lng(),
    startHeading * (Math.PI / 180) // then convert to radians
  ], [
    // end x, y, and heading
    end.lat(),
    end.lng(),
    endHeading * (Math.PI / 180) // then convert to radians
  ], turning_radius, step_size, (q, _) => {
    // callback
    newPoints.push([q[0], q[1]])
    return 0
  })
```
