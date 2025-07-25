<script setup>
import * as d3 from 'd3'
import { onMounted, ref, nextTick } from 'vue'

const svgRef = ref(null)
const containerRef = ref(null)

onMounted(async () => {
  // Wait for the next tick to ensure the container is rendered
  await nextTick()
  
  const nodes = [
    { id: "Coordinator", group: "coordinator" },
    { id: "Router 1", group: "router" },
    { id: "Router 2", group: "router" },
    { id: "Router 3", group: "router" },
    { id: "Router 4", group: "router" },
    { id: "End Device 1", group: "end" },
    { id: "End Device 2", group: "end" },
    { id: "End Device 3", group: "end" },
    { id: "End Device 4", group: "end" },
    { id: "End Device 5", group: "end" },
    { id: "End Device 6", group: "end" },
    { id: "End Device 7", group: "end" },
    { id: "End Device 8", group: "end" },
  ]

  const links = [
    { source: "Coordinator", target: "Router 1" },
    { source: "Coordinator", target: "Router 2" },
    { source: "Coordinator", target: "Router 3" },
    { source: "Router 1", target: "End Device 1" },
    { source: "Router 1", target: "End Device 2" },
    { source: "Router 2", target: "End Device 3" },
    { source: "Router 2", target: "End Device 4" },
    { source: "Router 3", target: "End Device 5" },
    { source: "Router 3", target: "End Device 6" },
    { source: "Router 2", target: "Router 4" },
    { source: "Router 4", target: "End Device 7" },
    { source: "Router 4", target: "End Device 8" },
  ]

  const color = {
    coordinator: "#e74c3c",
    router: "#e67e22",
    end: "#2ecc71",
  }

  // Get container dimensions with safety margins
  const container = containerRef.value
  const containerRect = container.getBoundingClientRect()
  
  // Use a more conservative height calculation to prevent overflow
  const maxHeight = Math.min(containerRect.height, window.innerHeight * 0.6)
  const width = containerRect.width
  const height = maxHeight

  const svg = d3.select(svgRef.value)
    .attr("width", width)
    .attr("height", height)
    .attr("viewBox", `0 0 ${width} ${height}`)

  const simulation = d3.forceSimulation(nodes)
    .force("link", d3.forceLink(links).id(d => d.id).distance(Math.min(width, height) * 0.15))
    .force("charge", d3.forceManyBody().strength(-Math.min(width, height) * 0.5))
    .force("center", d3.forceCenter(width / 2, height / 2))
    .force("collision", d3.forceCollide().radius(Math.min(width, height) * 0.04))

  const link = svg.append("g")
    .selectAll("line")
    .data(links)
    .join("line")
    .attr("stroke", "#999")

  const node = svg.append("g")
    .selectAll("circle")
    .data(nodes)
    .join("circle")
    .attr("r", Math.min(width, height) * 0.025)
    .attr("fill", d => color[d.group])

  // Add background rectangles for labels
  const labelBg = svg.append("g")
    .selectAll("rect")
    .data(nodes)
    .join("rect")
    .attr("width", d => d.id.length * Math.min(width, height) * 0.015 + Math.min(width, height) * 0.012)
    .attr("height", Math.min(width, height) * 0.032)
    .attr("rx", Math.min(width, height) * 0.006)
    .attr("fill", "rgba(255, 255, 255, 0.7)")
    .attr("stroke", "rgba(0, 0, 0, 0.1)")
    .attr("stroke-width", Math.min(width, height) * 0.001)

  const label = svg.append("g")
    .selectAll("text")
    .data(nodes)
    .join("text")
    .text(d => d.id)
    .attr("dy", -Math.min(width, height) * 0.04)
    .attr("text-anchor", "middle")
    .attr("font-size", `${Math.min(width, height) * 0.028}px`)

  simulation.on("tick", () => {
    // Constrain nodes to stay within SVG bounds with padding
    const padding = Math.min(width, height) * 0.08
    nodes.forEach(d => {
      d.x = Math.max(padding, Math.min(width - padding, d.x))
      d.y = Math.max(padding, Math.min(height - padding, d.y))
    })

    link
      .attr("x1", d => d.source.x)
      .attr("y1", d => d.source.y)
      .attr("x2", d => d.target.x)
      .attr("y2", d => d.target.y)

    node
      .attr("cx", d => d.x)
      .attr("cy", d => d.y)

    labelBg
      .attr("x", d => d.x - (d.id.length * Math.min(width, height) * 0.015 + Math.min(width, height) * 0.012) / 2)
      .attr("y", d => d.y - Math.min(width, height) * 0.067)

    label
      .attr("x", d => d.x)
      .attr("y", d => d.y)
  })

  // Handle window resize
  const handleResize = () => {
    const newContainerRect = container.getBoundingClientRect()
    const newMaxHeight = Math.min(newContainerRect.height, window.innerHeight * 0.6)
    const newWidth = newContainerRect.width
    const newHeight = newMaxHeight

    svg
      .attr("width", newWidth)
      .attr("height", newHeight)
      .attr("viewBox", `0 0 ${newWidth} ${newHeight}`)

    // Update simulation forces for new dimensions
    simulation
      .force("link", d3.forceLink(links).id(d => d.id).distance(Math.min(newWidth, newHeight) * 0.15))
      .force("charge", d3.forceManyBody().strength(-Math.min(newWidth, newHeight) * 0.5))
      .force("center", d3.forceCenter(newWidth / 2, newHeight / 2))
      .force("collision", d3.forceCollide().radius(Math.min(newWidth, newHeight) * 0.04))

    // Update node sizes
    node.attr("r", Math.min(newWidth, newHeight) * 0.025)

    // Update label backgrounds
    labelBg
      .attr("width", d => d.id.length * Math.min(newWidth, newHeight) * 0.015 + Math.min(newWidth, newHeight) * 0.012)
      .attr("height", Math.min(newWidth, newHeight) * 0.032)
      .attr("rx", Math.min(newWidth, newHeight) * 0.006)
      .attr("stroke-width", Math.min(newWidth, newHeight) * 0.001)

    // Update labels
    label
      .attr("dy", -Math.min(newWidth, newHeight) * 0.035)
      .attr("font-size", `${Math.min(newWidth, newHeight) * 0.028}px`)

    simulation.alpha(1).restart()
  }

  window.addEventListener('resize', handleResize)

  // Cleanup
  return () => {
    window.removeEventListener('resize', handleResize)
  }
})
</script>

<template>
  <div ref="containerRef" class="w-full h-80 flex justify-center overflow-hidden">
    <svg ref="svgRef" class="w-full h-full" preserveAspectRatio="xMidYMid meet"></svg>
  </div>
</template>
