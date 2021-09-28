class: center, middle

# ExecNodes

---

# Agenda

- Motivation
- Introduction to the `class`es
- Building and running an ExecPlan
- FAQs
- Eventually...

.note[Content warning: lotsa C++]

---

# Motivation


We need a way to specify arbitrarily complex computations
(queries) with reusable components.

One very common way to structure this is by defining a data
flow graph: data flows along edges and is processed at nodes.

---

# Motivation

<svg width="320pt" height="404pt"
 viewBox="0.00 0.00 388.02 404.00" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
<g id="graph0" class="graph" transform="scale(1 1) rotate(0) translate(4 400)">
<title>G</title>
<polygon fill="#ffffff" stroke="transparent" points="-4,4 -4,-400 384.0173,-400 384.0173,4 -4,4"/>
<!-- scan lineitem -->
<g id="node1" class="node">
<title>scan lineitem</title>
<ellipse fill="none" stroke="#000000" cx="62.2569" cy="-378" rx="62.0148" ry="18"/>
<text text-anchor="middle" x="62.2569" y="-373.8" font-family="Times,serif" font-size="14.00" fill="#000000">scan lineitem</text>
</g>
<!-- filter -->
<g id="node2" class="node">
<title>filter</title>
<ellipse fill="none" stroke="#000000" cx="86.2569" cy="-306" rx="29.6089" ry="18"/>
<text text-anchor="middle" x="86.2569" y="-301.8" font-family="Times,serif" font-size="14.00" fill="#000000">filter</text>
</g>
<!-- scan lineitem&#45;&gt;filter -->
<g id="edge1" class="edge">
<title>scan lineitem&#45;&gt;filter</title>
<path fill="none" stroke="#000000" d="M68.3132,-359.8314C70.9767,-351.8406 74.163,-342.2819 77.1065,-333.4514"/>
<polygon fill="#000000" stroke="#000000" points="80.4439,-334.5071 80.2858,-323.9134 73.8031,-332.2934 80.4439,-334.5071"/>
</g>
<!-- join -->
<g id="node3" class="node">
<title>join</title>
<ellipse fill="none" stroke="#000000" cx="184.2569" cy="-234" rx="27" ry="18"/>
<text text-anchor="middle" x="184.2569" y="-229.8" font-family="Times,serif" font-size="14.00" fill="#000000">join</text>
</g>
<!-- filter&#45;&gt;join -->
<g id="edge2" class="edge">
<title>filter&#45;&gt;join</title>
<path fill="none" stroke="#000000" d="M105.6186,-291.7751C120.5341,-280.8168 141.3184,-265.5467 157.7735,-253.4572"/>
<polygon fill="#000000" stroke="#000000" points="159.9433,-256.2062 165.9299,-247.4648 155.7988,-250.565 159.9433,-256.2062"/>
</g>
<!-- join again -->
<g id="node4" class="node">
<title>join again</title>
<ellipse fill="none" stroke="#000000" cx="231.2569" cy="-162" rx="49.2784" ry="18"/>
<text text-anchor="middle" x="231.2569" y="-157.8" font-family="Times,serif" font-size="14.00" fill="#000000">join again</text>
</g>
<!-- join&#45;&gt;join again -->
<g id="edge3" class="edge">
<title>join&#45;&gt;join again</title>
<path fill="none" stroke="#000000" d="M195.1578,-217.3008C200.8051,-208.6496 207.8305,-197.8873 214.1788,-188.1623"/>
<polygon fill="#000000" stroke="#000000" points="217.224,-189.9002 219.7594,-179.6132 211.3623,-186.0738 217.224,-189.9002"/>
</g>
<!-- filter again -->
<g id="node9" class="node">
<title>filter again</title>
<ellipse fill="none" stroke="#000000" cx="231.2569" cy="-90" rx="53.2645" ry="18"/>
<text text-anchor="middle" x="231.2569" y="-85.8" font-family="Times,serif" font-size="14.00" fill="#000000">filter again</text>
</g>
<!-- join again&#45;&gt;filter again -->
<g id="edge8" class="edge">
<title>join again&#45;&gt;filter again</title>
<path fill="none" stroke="#000000" d="M231.2569,-143.8314C231.2569,-136.131 231.2569,-126.9743 231.2569,-118.4166"/>
<polygon fill="#000000" stroke="#000000" points="234.757,-118.4132 231.2569,-108.4133 227.757,-118.4133 234.757,-118.4132"/>
</g>
<!-- scan orders -->
<g id="node5" class="node">
<title>scan orders</title>
<ellipse fill="none" stroke="#000000" cx="197.2569" cy="-378" rx="54.9752" ry="18"/>
<text text-anchor="middle" x="197.2569" y="-373.8" font-family="Times,serif" font-size="14.00" fill="#000000">scan orders</text>
</g>
<!-- project -->
<g id="node6" class="node">
<title>project</title>
<ellipse fill="none" stroke="#000000" cx="184.2569" cy="-306" rx="37.6986" ry="18"/>
<text text-anchor="middle" x="184.2569" y="-301.8" font-family="Times,serif" font-size="14.00" fill="#000000">project</text>
</g>
<!-- scan orders&#45;&gt;project -->
<g id="edge4" class="edge">
<title>scan orders&#45;&gt;project</title>
<path fill="none" stroke="#000000" d="M193.9765,-359.8314C192.5861,-352.131 190.9329,-342.9743 189.3877,-334.4166"/>
<polygon fill="#000000" stroke="#000000" points="192.8028,-333.6322 187.5816,-324.4133 185.9142,-334.8761 192.8028,-333.6322"/>
</g>
<!-- project&#45;&gt;join -->
<g id="edge5" class="edge">
<title>project&#45;&gt;join</title>
<path fill="none" stroke="#000000" d="M184.2569,-287.8314C184.2569,-280.131 184.2569,-270.9743 184.2569,-262.4166"/>
<polygon fill="#000000" stroke="#000000" points="187.757,-262.4132 184.2569,-252.4133 180.757,-262.4133 187.757,-262.4132"/>
</g>
<!-- scan customers -->
<g id="node7" class="node">
<title>scan customers</title>
<ellipse fill="none" stroke="#000000" cx="310.2569" cy="-306" rx="69.5216" ry="18"/>
<text text-anchor="middle" x="310.2569" y="-301.8" font-family="Times,serif" font-size="14.00" fill="#000000">scan customers</text>
</g>
<!-- aggregate -->
<g id="node8" class="node">
<title>aggregate</title>
<ellipse fill="none" stroke="#000000" cx="294.2569" cy="-234" rx="48.6346" ry="18"/>
<text text-anchor="middle" x="294.2569" y="-229.8" font-family="Times,serif" font-size="14.00" fill="#000000">aggregate</text>
</g>
<!-- scan customers&#45;&gt;aggregate -->
<g id="edge6" class="edge">
<title>scan customers&#45;&gt;aggregate</title>
<path fill="none" stroke="#000000" d="M306.2195,-287.8314C304.5083,-280.131 302.4735,-270.9743 300.5717,-262.4166"/>
<polygon fill="#000000" stroke="#000000" points="303.9348,-261.4159 298.3488,-252.4133 297.1015,-262.9344 303.9348,-261.4159"/>
</g>
<!-- aggregate&#45;&gt;join again -->
<g id="edge7" class="edge">
<title>aggregate&#45;&gt;join again</title>
<path fill="none" stroke="#000000" d="M279.0064,-216.5708C271.1906,-207.6385 261.5369,-196.6056 252.9595,-186.8029"/>
<polygon fill="#000000" stroke="#000000" points="255.5861,-184.4897 246.367,-179.2687 250.3181,-189.0993 255.5861,-184.4897"/>
</g>
<!-- write to disk -->
<g id="node10" class="node">
<title>write to disk</title>
<ellipse fill="none" stroke="#000000" cx="231.2569" cy="-18" rx="59.1276" ry="18"/>
<text text-anchor="middle" x="231.2569" y="-13.8" font-family="Times,serif" font-size="14.00" fill="#000000">write to disk</text>
</g>
<!-- filter again&#45;&gt;write to disk -->
<g id="edge9" class="edge">
<title>filter again&#45;&gt;write to disk</title>
<path fill="none" stroke="#000000" d="M231.2569,-71.8314C231.2569,-64.131 231.2569,-54.9743 231.2569,-46.4166"/>
<polygon fill="#000000" stroke="#000000" points="234.757,-46.4132 231.2569,-36.4133 227.757,-46.4133 234.757,-46.4132"/>
</g>
</g>
</svg>

???

What's a stream mean?
In our case, it means that a node has some data and pushes
batches of that that data into a receiving node (push model).

Streaming is critical because we can throttle
how much data is in memory at a time- so we can tune batch
size for performance at a single node independent of any
other nodes.

The flow of data makes a very convenient interface for
compartmentalization, too:
- Nodes with differing functionality don't need to be
  tied together in any way with nodes at other places in
  the graph.
- Streams are well defined across machine boundaries,
  so we can distribute the graph as well (for example
  have basic scan/filter/project close to source data
  while then having those stream to a join node on another
  box)
- Streams are also well defined across backend boundaries-
  a GPU backed node can totally stream to a CPU backed node
  and vice versa.

You may have encountered pull-model streams before, which are
the opposite: each node requests data as needed from its
upstream nodes.

Each of these has strengths and weaknesses, but
We use push model because newer data base systems have reported
better cache coherence than using pull model.
https://arxiv.org/pdf/1610.09166.pdf

SHAIKHHA, A., DASHTI, M., & KOCH, C. (2018). Push versus pull-based loop fusion in query engines. Journal of Functional Programming, 28. https://doi.org/10.1017/s0956796818000102

- filter: comment does *not* indicate a special category of product
- join: on customer id so that we're looking at an order and the user who made it
- count distinct customers

```
digraph G {

  "scan lineitem"
    -> "filter"
    -> "join"
    -> "join again";

  "scan orders"
    -> "project"
    -> "join";

  "scan customers"
    -> "aggregate"
    -> "join again"
    -> "filter again"
    -> "write to disk";

}
```

---

# The `class`es

quick overview:

#### src/arrow/compute/exec/exec_plan.h

```
// each node in the graph is an implementation of ExecNode
class ExecNode;

// all the nodes in the graph are owned by a single ExecPlan
class ExecPlan;

// nodes are constructed by factories in an ExecFactoryRegistry
class ExecFactoryRegistry;

// dplyr-inspired helper for efficiently constructing ExecPlans
struct Declaration;
```

#### src/arrow/compute/exec/options.h

```
// nodes are parameterized by ExecNodeOptions
class ExecNodeOptions;
```

---

# The `class`es

```md
class ExecNode;
class ExecPlan;
class ExecFactoryRegistry;
struct Declaration;
class ExecNodeOptions;
```

Before we go further: what's a batch?

```
// - lightweight
// - for use in compute:: only.
// - supports scalar "columns" (a single literal value)
// - has execution-relevant properties like guarantees
namespace arrow::compute {
  struct ExecBatch;
}

// - abstract base class (interface)
// - very public, exposed in all bindings
// - may eventually lazily materialize its columns for dataframiness
namespace arrow {
  class RecordBatch;
}
```

These are interconvertible to each other (and to/from `cudf::table`).

???

Both of these are single chunk- if you've got a column of 1k float32s
then that will be stored in a single contiguous buffer of 4kBytes.

- arrow::compute::ExecBatch is intended to be an extremely lightweight chunk of data, cheaply transmissible along edges of a compute graph. They support columns which are Scalars (a single literal value) in addition to Arrays, and carry execution-relevant sidecar properties like a guaranteed-true-filter for Expression simplification and a SelectionVector indicating delayed filtration.
- arrow::RecordBatch is an abstract base class. Although there is currently only one implementation (SimpleRecordBatch) there may eventually be fancier ones, such as one which lazily materializes its columns. It is much less cheap. arrow::Tables are also abstract base classes with the same motivation and unlike batches their columns are not required to be a single chunk of array.

---

# The `class`es - `ExecNode`

while the graph is running, each instance of ExecNode:

1. has batches pushed to it from inputs and processes them
  - unless it's a source
  - might be stateful or stateless
  - responsible for its own sync
2. pushes batches to outputs
  - unless it's a sink

???

maps onto a Blazing kernel

    - receives batches (unless it's a source) then does something then
      emits batches (unless it's a sink)
      - for example a FilterNode receives batches, drops some
        rows, then passes it along
      - for example an AggregateNode receives batches, updates
        sums, then emits results when the input completes

---

```
// a basic ExecNode which passes all input batches through unchanged
class PassthruNode : public ExecNode {
 public:
  PassthruNode(ExecNode* input, const ExampleNodeOptions&)
      : ExecNode(/*plan=*/input->plan(), /*inputs=*/{input},
                 /*input_labels=*/{"passed_through"},
                 /*output_schema=*/input->output_schema(), /*num_outputs=*/1) {}

  const char* kind_name() const override { return "PassthruNode"; }

  Status StartProducing() override { return Status::OK(); }
  void StopProducing() override {}

  void ResumeProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
  void PauseProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
  void StopProducing(ExecNode* output) override { inputs_[0]->StopProducing(this); }
  
  void InputReceived(ExecNode* input, ExecBatch batch) override {
    outputs_[0]->InputReceived(this, batch);
  }
  void ErrorReceived(ExecNode* input, Status error) override {
    outputs_[0]->ErrorReceived(this, error);
  }
  void InputFinished(ExecNode* input, int total_batches) override {
    outputs_[0]->InputFinished(this, total_batches);
  }

  Future<> finished() override { return inputs_[0]->finished(); }
};
```

???

This isn't really relevant to people working on bindings since you'd
never have to implement an ExecNode (just construct a graph of them).

However I thought I'd walk everybody
through a basic implementation anyway just to show that it's a pretty
straightforward class.

---

```
// a basic ExecNode which passes all input batches through unchanged
class PassthruNode : public ExecNode {
 public:
  PassthruNode(ExecNode* input, const ExampleNodeOptions&)
      : ExecNode(/*plan=*/input->plan(), /*inputs=*/{input},
                 /*input_labels=*/{"passed_through"},
                 /*output_schema=*/input->output_schema(), /*num_outputs=*/1) {}

  const char* kind_name() const override { return "PassthruNode"; }

  Status StartProducing() override { return Status::OK(); }
  void StopProducing() override {}

  void ResumeProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
  void PauseProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
  void StopProducing(ExecNode* output) override { inputs_[0]->StopProducing(this); }

* void InputReceived(ExecNode* input, ExecBatch batch) override {
*   outputs_[0]->InputReceived(this, batch);
* }
  void ErrorReceived(ExecNode* input, Status error) override {
    outputs_[0]->ErrorReceived(this, error);
  }
  void InputFinished(ExecNode* input, int total_batches) override {
    outputs_[0]->InputFinished(this, total_batches);
  }

  Future<> finished() override { return inputs_[0]->finished(); }
};
```

???

Note that this is a push model interface:
1. an input pushes to this node via the `InputReceived` method
2. does work on the batch and pushes to its output
   - for this example we don't do any work
   - for a FilterNode this is where rows would be dropped
4. alternatively, we might push an error instead using ErrorReceived

---

```
// a basic ExecNode which passes all input batches through unchanged
class PassthruNode : public ExecNode {
 public:
  PassthruNode(ExecNode* input, const ExampleNodeOptions&)
      : ExecNode(/*plan=*/input->plan(), /*inputs=*/{input},
                 /*input_labels=*/{"passed_through"},
                 /*output_schema=*/input->output_schema(), /*num_outputs=*/1) {}

  const char* kind_name() const override { return "PassthruNode"; }

  Status StartProducing() override { return Status::OK(); }
  void StopProducing() override {}

  void ResumeProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
  void PauseProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
  void StopProducing(ExecNode* output) override { inputs_[0]->StopProducing(this); }

  void InputReceived(ExecNode* input, ExecBatch batch) override {
    outputs_[0]->InputReceived(this, batch);
  }
  void ErrorReceived(ExecNode* input, Status error) override {
    outputs_[0]->ErrorReceived(this, error);
  }
* void InputFinished(ExecNode* input, int total_batches) override {
*   outputs_[0]->InputFinished(this, total_batches);
* }

  Future<> finished() override { return inputs_[0]->finished(); }
};
```

???

Slightly confusing name here, but this method is used to signal how many
batches will ultimately arrive. *It is not sequenced at all.*
- For this example node, since the number of batches passed through will be
  identical to the number of batches received, we can simply forward that total
  count
- For an aggregate node, we need to wait until we've received all batches of
  input since the number of groups (and therefore the number of batches of output)
  is dependent on each row from the input
  - Unless we're doing scalar aggregation, in which case we'll always output a single row

---

```
// a basic ExecNode which passes all input batches through unchanged
class PassthruNode : public ExecNode {
 public:
  PassthruNode(ExecNode* input, const ExampleNodeOptions&)
      : ExecNode(/*plan=*/input->plan(), /*inputs=*/{input},
                 /*input_labels=*/{"passed_through"},
                 /*output_schema=*/input->output_schema(), /*num_outputs=*/1) {}

  const char* kind_name() const override { return "PassthruNode"; }

  Status StartProducing() override { return Status::OK(); }
  void StopProducing() override {}

* void ResumeProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
* void PauseProducing(ExecNode* output) override { inputs_[0]->PauseProducing(this); }
* void StopProducing(ExecNode* output) override { inputs_[0]->StopProducing(this); }

  void InputReceived(ExecNode* input, ExecBatch batch) override {
    outputs_[0]->InputReceived(this, batch);
  }
  void ErrorReceived(ExecNode* input, Status error) override {
    outputs_[0]->ErrorReceived(this, error);
  }
  void InputFinished(ExecNode* input, int total_batches) override {
    outputs_[0]->InputFinished(this, total_batches);
  }

  Future<> finished() override { return inputs_[0]->finished(); }
};
```

???

nodes which receive batches from passthru may request that the passthru pause,
resume, or stop (which for this example just forwards those to its only input)

Note that most of the methods we've looked at so far are scoped to an edge
of the graph- they are called exclusively by other nodes which are either
one step upstream or downstream from `this` node.
  - This includes error reporting != stopping; errors pass through the graph
    alongside batches instead of stopping the whole graph

ExecNodes need an explicit start from outside the graph, and *may* be
The StartProducing and StopProducing methods are invoked by the ExecPlan
when the entire graph gets going or shuts down


---

# The `class`es - `ExecNode`

ExecNode's extra properties:

```
class ExecNode {
  // ...

  const std::vector<ExecNode*>& inputs() const;
  const std::vector<std::string>& input_labels() const;

  const std::vector<ExecNode*>& outputs() const;

  const std::shared_ptr<Schema>& output_schema() const;

  ExecPlan* plan();

  const std::string& label() const;
  void SetLabel(std::string label);

  std::string ToString() const;
```

???

Note that you could use `inputs` and `outputs` to walk the graph
if you needed to do some reflection-y thing on it.

Note that each `ExecNode` has a single output schema

---

# The `class`es - `ExecPlan`

- keeps ExecNodes alive
- provides convenience methods for `"start all nodes!"`,
  `"stop all nodes!"`, and `"have all nodes completed?"`

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

// add nodes to your ExecPlan
// ... (coming soon)

// start all nodes in the graph
ASSERT_OK(plan->StartProducing());

if (need_stop) {
  plan->StopProducing();
}

// wait for the plan to finish
plan->finished()->Wait();
```

???

Note: R users will need to use the gc_exec_context() instead
or risk spurious OutOfMemory errors.

---

# The `class`es - `ExecFactoryRegistry`

- `ExecNode` subclasses are not constructed directly
- factories for each subclass are provided in the registry

```
// get the factory for "filter" nodes:
auto make_filter = *default_exec_factory_registry()->GetFactory("filter");

// factories take three arguments:
ExecNode* filter_node = *make_filter(
  // the ExecPlan which should own this node
  plan.get(),

  // nodes which will send batches to this node (inputs)
  {scan_node},

  // parameters unique to "filter" nodes
  FilterNodeOptions{filter_expression});
```

???

None of the concrete implementations of `ExecNode` are exposed
in headers, so they can't be constructed directly outside the
Translation Unit where they are defined.
Instead, factories to create them are added to a registry.

"why a registry?"

- Decouple implementations from consumers of the interface
  (prime example: we have two classes for scalar and grouped aggregate and
   potentially multiple coming up for hash join)
- Optimization
- Easier composition with out-of-library extensions like "scan" nodes
- More consitent construction for the bindings

---

# The `class`es - `ExecFactoryRegistry`

`src/arrow/compute/exec/exec_plan.h`


- `ExecNode` subclasses are not constructed directly
- factories for each subclass are provided in the registry

#### .note[Don't use factories directly; we have helper functions:]

```
ExecNode* filter_node = *MakeExecNode("filter",

  // the ExecPlan which should own this node
  plan.get(),

  // nodes which will send batches to this node (inputs)
  {scan_node},

  // parameters unique to "filter" nodes
  FilterNodeOptions{filter_expression});
```

---

# How to build an ExecPlan

.note[The following will be superceded by Compute IR.]

---

# How to build an ExecPlan

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

// add a source node which pulls batches from a RecordBatchReader
std::shared_ptr<RecordBatchReader> reader = GetStreamOfBatches();
ExecNode* source_node = *MakeExecNode("source",
                                      plan.get(),
                                      {},
                                      SourceNodeOptions::FromReader(
                                          reader,
                                          GetCpuThreadPool()));

// add a filter node which excludes rows from source_node's batches
ExecNode* filter_node = *MakeExecNode("filter",
                                      plan.get(),
                                      {source_node},
                                      FilterNodeOptions{
                                        greater(field_ref("score"), literal(3))
                                      });

// add a project node which materializes new columns based on filter_node's output
ExecNode* project_node = *MakeExecNode("project", /*...*/);

// add a write node which collects the batches produced by the pipeline
MakeExecNode("write", /*...*/);
```

.footnote[FromReader is in [PR#11032](https://github.com/apache/arrow/pull/11032)
at the moment, I've included it here for brevity]

???

Note: a source node can be made from anything which resembles a stream
of batches. That PR is wrapping a stream of data from DuckDB as a source
node! Give us a stream, we can plug it into an ExecPlan.

---

# How to build an ExecPlan

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

// add a source node which pulls batches from a RecordBatchReader
ExecNode* source_node = *MakeExecNode("source", /* ... */);

// add a filter node which excludes rows from source_node's batches
ExecNode* filter_node = *MakeExecNode("filter", /* ... */);

// add a project node which materializes a new column from filter_node's output
ExecNode* project_node = *MakeExecNode("project",
                                       plan.get(),
                                       {filter_node},
                                       ProjectNodeOptions{
                                         {add(field_ref("score"), literal(1))},
                                         {"score + 1"}
                                       });

// add a write node which collects the batches produced by the pipeline
MakeExecNode("write",
             plan.get(),
             {project_node},
             WriteNodeOptions{.base_dir = "/dat", /*...*/});
```

.footnote[[WriteNode is in PR#11017](https://github.com/apache/arrow/pull/11017)
at the moment, I've included it here for brevity]

???

Note: a sink node is any node which has no outputs;
batches which arrive at such a node leave the graph in some way.
In this example the batches pushed to our sink node are
dumped to disk. However we could just as easily keep those
in memory as a Table or a RecordBatchReader to be used by some
non-ExecNode consumer.

Thus:
an ExecPlan can be consumed by anything which can utilize a stream of batches,
and anything which can consume a stream of batches can plug into an ExecPlan.

---

# How to build an ExecPlan

And for even less boilerplate, we have dplyr-inspired `Declaration`s:

```
// construct an ExecPlan first to hold your nodes
std::shared_ptr<ExecPlan> plan = *ExecPlan::Make(default_exec_context());

std::shared_ptr<RecordBatchReader> reader = GetStreamOfBatches(), sink_reader;
ASSERT_OK(Declaration::Sequence(
              {
                  {"source", SourceNodeOptions::FromReader(
                       reader,
                       GetCpuThreadPool())},
                  {"filter", FilterNodeOptions{
                       greater(field_ref("score"), literal(3))}},
                  {"project", ProjectNodeOptions{
                       {add(field_ref("score"), literal(1))},
                       {"score + 1"}}},
                  {"write", WriteNodeOptions{.base_dir = "/dat", /*...*/}},
              })
              .AddToPlan(plan.get()));
```

(These are used in the c++ unit tests and in the implementation of `Scanner`)

---

# How to build an ExecPlan

- What about nodes with multiple inputs?

```
Declaration left{"source", SourceNodeOptions::FromReader(
   customer_reader,
   GetCpuThreadPool())};

Declaration right = Declaration::Sequence({
        {"source", SourceNodeOptions::FromReader(
             customer_reader, GetCpuThreadPool())}
        {"filter", FilterNodeOptions{
             not_like(field_ref("o_comment"), R"(\w+\b\w+)")}}});

ASSERT_OK(Declaration::Sequence(
              {
*                 {"hashjoin", {left, right}, HashJoinNodeOptions{
                      /* join kind, join fields, ... */}},

                  {"aggregate", AggregateNodeOptions{
                      /*aggregations=*/{{"hash_count"}},
                      /*targets=*/{"o_orderkey"},
                      /*names=*/{"c_count"},
                      /*keys=*/{"c_custkey"},
                      },
                  //...
              })
              .AddToPlan(plan.get()));
```

???

The hash join node is in a PR
https://github.com/apache/arrow/pull/11150

---

# How to build an ExecPlan

- What about datasets?

Datasets can be used to produce scan nodes.

```
arrow::dataset::internal::Initialize();

std::shared_ptr<Dataset> dataset = GetDataset();

ASSERT_OK(Declaration::Sequence(
              {
                  {"scan", ScanNodeOptions{
*                    dataset,
                     /* push down predicate, projection, ... */},
                  {"filter", FilterNodeOptions{/* ... */}},
                  // ...
              })
              .AddToPlan(plan.get()));
```

Datasets may be scanned multiple times; just make multiple scan
nodes from that dataset. (Useful for a self-join, for example.)

???
Scan nodes are, which act like source nodes which read from the dataset instead
of from a reader.
(It might be useful to think of a scan node as shorthand for constructing
a `Scanner` which scans your dataset, getting a `Reader` from the dataset,
then wrapping that `Reader` into a source node)

By contrast: RecordBatchReader is not reusable!

Note that producing two scan nodes will perform all reads and decodes twice-
we don't support splitting a stream of batches yet.

---

# FAQs

- How does this relate to an `arrow::compute::Kernel`?

.note[Confusion warning:
BlazingSQL had (has?) a class named Kernel, but it was most analagous
to ExecNode.]

`arrow::compute::Kernel`s represent a different unit of reusable computation.
- avoid/defer allocation if possible
- not internally thread safe
- minimal functionality
- targeted for processing single batches

`arrow::compute::Kernel`s are *.note[used by]* ExecNode implementations

???

  (favorite example: one of the addition kernels adds two buffers of
   `int32_t` into a consumer-preallocated buffer of `int32_t`)

In short, `arrow::compute::Kernel`s are mostly *.note[used by]*
ExecNode implementations. For example, `FilterNode` uses potentially
many kernels to evaluate filter conditions and others to materialize
batches with some rows excluded by that filter condition.

---

# FAQs

- How does this relate to an `arrow::compute::Function`?
- How does this relate to an `arrow::compute::Expression`?

`arrow::compute::Function`s are a named collection of related kernels.

`arrow::compute::Expression`s are a bundle of function name, arguments, ...

The process of selecting the correct/optimal kernel is called "dispatch".

???

  (favorite example: the "add" function
   contains one kernel for each input data type)

... everything needed to dispatch and execute.

ExecNodes are usually configured using expressions.
As ExecNodes are initialized, they perform dispatch (once, before
running the graph) to select the correct kernels.

---

# FAQs

- Can I make a Dataset which wraps a RecordBatchReader?

No.

Datasets are *re*scannable, RecordBatchReaders are single-use.

Make a scan node instead!

???

Related: can I make a Dataset which wraps an ExecPlan (also no).
Keep in mind that an ExecPlan maps more-or-less onto a
RecordBatchReader/stream of results- it's not reusable.

---

# FAQs

- What about multiple outputs?

We don't currently have any ExecNodes like that.
There's active debate over whether we want to.

.note[out of scope]
---

# FAQs

- How are batches ordered?

Inside an ExecPlan, they are not ordered.

Dataset scans can cheat this slightly.

`OrderBySinkNode` is the ExecNode which corresponds to
SQL's `ORDER BY`.

We may need to change this; some aggregates and window functions
require ordering of their input.

> [Active debate currently being led by Weston Pace.](https://docs.google.com/document/d/1MfVE9td9D4n5y-PTn66kk4-9xG7feXs1zSFf-qxQgPs/edit#)
???

Tagging breaks down some when we split batches for fan out

OrderBySinkNode sorts the batches *as they leave the graph*.

Our interface for reusability is the stream of batches,
which is unordered.

---

# Eventually...

  - Taskify (ARROW-13576)
  - Backpressure/spill-to-disk/out-of-core
  - Fan out execution of expressions ala HyPer
  - Maybe allow ordering along some edges of the graph
  - Maybe multiple outputs
  - Compute IR -> ExecPlan (ARROW-14074)
  - GPU backed ExecNodes
  - Writing to a dataset (ARROW-13542)

???

No more explicit construction. *Everybody* should produce IR
    and have that mapped onto ExecNodes.
