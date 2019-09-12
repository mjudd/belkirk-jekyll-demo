---
product:
- Co>Operating System
topics: []
title: How a Graph Runs
blurb: |-
  What happens on the Co>Operating System run host and processing hosts after
  you click the run button to start a graph?
video: ''
poster: "/uploads/2019/09/11/vidframe_cos_how_a_graph_runs_196x128.png"
transcript: |
  <br/>
  <v Narrator>
  <b>Narrator</b>: When using Ab Initio products, you build applications with graphs.
  The execution of a graph is a job.

  To execute or run a graph, click the run button in the GDE.
  Or you can invoke the \$air sandbox run command from the command line, specifying the name of a graph or a pset.

  > The screen shows syntax: \$air sandbox run mygraph.mp [00:20]

  On a production system, you set up scheduled events that run the jobs programmatically, either through the Control Center or through a third-party scheduling utility.

  So what happens when you click the run button in the GDE?

  1. The GDE sends a command to the Co>Operating System on the run host.

  1. The Co>Operating system generates a .ksh script, and then the script starts running.

  1. The .ksh script runs a set of Co>Operating System programs that first define the structure of the graph, and then start a set of processes.

  The first process that starts is the launcher. The launcher manages the actual execution of the graph. Each job only has one launcher process, no matter how many partitions the job is using, or how many computers the job is running on. The launcher starts one or more agent processes on each processing computer, and usually the run host is also a processing computer.

  > The screen shows the Launcher represented as a rocket firing; agents represented as humans wearing dark glasses and fedoras; and computers represented as rectangular boxes, one labeled "run host." [01:12]

  {% timing "Communication during a successful graph run", "01:18" %}

  Let’s watch a simple serial graph run.

  1. The launcher starts running, and then it starts an agent.

  1. The agent then starts the component processes.

  > The screen shows data flowing from an input file, through Components A, B, and C in turn, to an output file. [01:33]

  3. When a component finishes processing the data, it tells the agent that it’s finished, and then stops.

  1. When all the components have finished running and have reported back to the agent, the agent reports back to the launcher.

  1. The launcher stops the agent, reports the job status back to the GDE, and then stops running.

  But sometimes jobs fail. Let’s rewind a bit.

  {% timing "Communication during a failed graph run", "02:04" %}

  Let’s say that Component C runs into a problem.

  Component C sends an error message to the agent, and stops.

  > The screen shows Component C to agent: "Oops. I’ve been asked to divide by zero."

  The agent reports the message back to the launcher.

  > The screen shows Agent to Launcher: "Component C cannot divide by zero."

  The launcher instructs the agent to stop the job.

  > The screen shows Launcher to Agent: "Shut down all other components."

  And then the launcher sends the error message back to the GDE.

  In this case, you would examine the error message, troubleshoot the problem, and try running the graph again.

  We just showed a job that ran on a single server. What happens when a job is distributed across multiple servers?

  {% timing "How a job runs across multiple server", "02:42" %}

  The launcher still runs the show.

  1. This time, the launcher starts two agents, one on the run host and the second one on the remote processing computer.

  1. Each agent starts a set of components.

  1. The components on each computer start processing records.

  > The screen shows data flowing from an input file, through two parallel flows (components A1, B1, and C1, and components A2, B2, and C2) to a single output file. [03:01]

  4. When a component finishes processing its records, it reports back to its agent and then stops.

  1. When all the components that were started by a specific agent have finished running, and have reported back to that agent, the agent reports back to the launcher.

  1. After both agents have reported back to the launcher, the launcher stops each agent, reports the job status back to the GDE, and stops.

  > The screen shows computer screen with checkmark, "Done!" [3:30]

  Let’s rewind again, and watch what happens when this kind of job fails.

  Component C1 reports its error to Agent 1, and stops. Agent 1 reports the message back to the launcher. But Agent 2 and the components on the remote processing computer are still running, so the launcher tells Agent 2 to stop its components, too. Then the launcher sends an error message back to the GDE, and stops running.

  > The screen shows Launcher to GDE: “We have a problem!” [3:56]

  Once again, you would examine the message, fix the problem, and rerun the graph.

  And that’s how a graph runs.

---
