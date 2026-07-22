## Problem
The Skyline Problem is a geometric problem that involves finding the skyline of a city given a list of buildings with their positions and heights. The skyline is a list of (x, h) pairs representing the x-coordinate and height of the skyline at that point. Each building is represented by a tuple of three integers (left, right, height), where left and right are the x-coordinates of the left and right edges of the building, and height is the height of the building. The goal is to find the skyline of the city, which is the outer contour of the buildings.

## Examples
* Input: `buildings = [(2, 9, 10), (3, 7, 15), (5, 12, 12), (15, 20, 10), (19, 24, 8)]`
  Output: `[(2, 10), (3, 15), (7, 12), (12, 0), (15, 10), (20, 8), (24, 0)]`
* Input: `buildings = [(0, 2, 3), (2, 5, 3), (6, 9, 3)]`
  Output: `[(0, 3), (5, 0), (6, 3), (9, 0)]`

## Approach
To solve this problem, we can use a sweep line approach. The idea is to imagine a vertical line sweeping from left to right across the city. As the line sweeps, it encounters the left and right edges of the buildings, and the height of the skyline changes accordingly. We can use a priority queue to keep track of the current height of the skyline at each point.

Here are the steps to solve the problem:
1. Sort the buildings by their left x-coordinates.
2. Initialize an empty priority queue to store the current height of the skyline at each point.
3. Initialize an empty list to store the skyline.
4. Iterate over the sorted buildings. For each building, add its height to the priority queue at its left x-coordinate, and remove its height from the priority queue at its right x-coordinate.
5. After processing each building, check if the height of the skyline has changed. If it has, add the current x-coordinate and height to the skyline list.

## Solution
```python
import heapq

def get_skyline(buildings):
    # Sort the buildings by their left x-coordinates
    events = [(L, -H, R) for L, R, H in buildings] + [(R, 0, "None") for _, R, _ in buildings]
    events.sort()

    # Initialize an empty priority queue to store the current height of the skyline at each point
    live = [(0, float("inf"))]
    heapq.heapify(live)

    # Initialize an empty list to store the skyline
    skyline = []

    # Iterate over the sorted events
    for x, negH, R in events:
        # If the event is a left edge, add its height to the priority queue
        if negH:
            heapq.heappush(live, (negH, R))
        # If the event is a right edge, remove its height from the priority queue
        else:
            live = [(h, r) for h, r in live if r != R]

        # Get the current height of the skyline
        currH = -live[0][0]

        # If the height of the skyline has changed, add the current x-coordinate and height to the skyline list
        if not skyline or currH != skyline[-1][1]:
            skyline.append([x, currH])

    return skyline
```

## Complexity
- Time: O(n log n) — The time complexity is O(n log n) because we are sorting the buildings and using a priority queue to keep track of the current height of the skyline. The sorting operation takes O(n log n) time, and the priority queue operations take O(log n) time each.
- Space: O(n) — The space complexity is O(n) because we are storing the skyline and the priority queue, both of which can have up to n elements in the worst case.

## Key Insight
The core trick to solving this problem is to use a sweep line approach with a priority queue to keep track of the current height of the skyline at each point, allowing us to efficiently calculate the outer contour of the buildings.