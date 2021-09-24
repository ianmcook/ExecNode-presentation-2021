class: center, middle

# ExecNodes

---

# Agenda

- Motivation
- Introduction to the `class`es
- Building and running an ExecPlan
- FAQs
- Eventually...

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

```
// each node in the graph is an implementation of ExecNode
class ExecNode;

// all the nodes in the graph are owned by a single ExecPlan
class ExecPlan;

// nodes are constructed by factories in an ExecFactoryRegistry
class ExecFactoryRegistry;

// nodes are parameterized by ExecNodeOptions
class ExecNodeOptions;
```

---

# The `class`es

```md
class ExecNode;
class ExecPlan;
class ExecFactoryRegistry;
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
namespace compute {
  class RecordBatch;
}
```

These are interconvertible to each other (and to/from `cudf::table`).

???

- arrow::compute::ExecBatch is intended to be an extremely lightweight chunk of data, cheaply transmissible along edges of a compute graph. They support columns which are Scalars (a single literal value) in addition to Arrays, and carry execution-relevant sidecar properties like a guaranteed-true-filter for Expression simplification and a SelectionVector indicating delayed filtration.
- arrow::RecordBatch is an abstract base class. Although there is currently only one implementation (SimpleRecordBatch) there may eventually be fancier ones, such as one which lazily materializes its columns. It is much less cheap. arrow::Tables are also abstract base classes with the same motivation and unlike batches their columns are not required to be a single chunk of array.

---

# The `class`es - `ExecNode`

`src/arrow/compute/exec/exec_plan.h`

while the graph is running, instances of ExecNode:

1. receive batches from inputs and process them
  - unless it's a source
  - might be stateful or stateless, responsible for its own sync
2. emit batches to outputs
  - unless it's a sink

???

maps onto a Blazing kernel

    - receives batches (unless it's a source) then does something then
      emits batches (unless it's a sink)
      - for example a FilterNode receives batches, drops some
        rows, then passes it along
      - for example an AggregateNode receives batches, updates
        sums, then emits results when the input completes
    - note that it's a graph node: you can look at the inputs
      and outputs vector to walk upstream or downstream in the
      graph
    - lifecycle methods:
      - need explicit start from outside the graph
      - can be stopped from outside the graph (for example, if the
      - can be stopped by an output (which doesn't need more batches)
      - error reporting != stopping; errors pass through the graph
        alongside batches instead of breaking everything.
    - note that each ExecNode has a single output schema

---

```
// a basic ExecNode which ignores all input batches
class ExampleNode : public cp::ExecNode {
 public:
  ExampleNode(ExecNode* input, const ExampleNodeOptions&)
      : ExecNode(/*plan=*/input->plan(), /*inputs=*/{input},
                 /*input_labels=*/{"ignored"},
                 /*output_schema=*/input->output_schema(), /*num_outputs=*/1) {}

  const char* kind_name() const override { return "ExampleNode"; }

  arrow::Status StartProducing() override {
    outputs_[0]->InputFinished(this, 0);
    return arrow::Status::OK();
  }

  void ResumeProducing(ExecNode* output) override {}
  void PauseProducing(ExecNode* output) override {}

  void StopProducing(ExecNode* output) override { inputs_[0]->StopProducing(this); }
  void StopProducing() override { inputs_[0]->StopProducing(); }

  void InputReceived(ExecNode* input, cp::ExecBatch batch) override {}
  void ErrorReceived(ExecNode* input, arrow::Status error) override {}
  void InputFinished(ExecNode* input, int total_batches) override {}

  arrow::Future<> finished() override { return inputs_[0]->finished(); }
};
```

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

None of the concrete implementations of `ExecNode` are exposed
in headers, so they can't be constructed directly outside the
Translation Unit where they are defined.
Instead, factories to create them are added to a registry.

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

"why a registry?"

- Decouple implementations from consumers of the interface
- Optimization
- Easier composition with out-of-library extensions like "scan" nodes
- More consitent construction for the bindings

---

# The `class`es - `ExecFactoryRegistry`

None of the concrete implementations of `ExecNode` are exposed
in headers, so they can't be constructed directly outside the
Translation Unit where they are defined.
Instead, factories to create them are added to a registry.

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

// add a sink node which collects the batches produced by the pipeline
ExecNode* sink_node = *MakeExecNode("sink", /*...*/);
```

.footnote[FromReader is in [PR#11032](https://github.com/apache/arrow/pull/11032) at the moment, I've included it here for brevity]

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

// add a sink node which collects the batches produced by the pipeline
std::shared_ptr<RecordBatchReader> sink_reader;
ExecNode* sink_node = *MakeExecNode("sink",
                                    plan.get(),
                                    {project_node},
                                    SinkNodeOptions::IntoReader(&sink_reader));
```

.footnote[IntoReader is not in any PR yet]

???

Note: a sink node produces a stream of batches, so it can
be consumed by anything which can utilize a stream of batches.
Anything which can consume a stream can plug into an ExecPlan.

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
                  {"sink", SinkNodeOptions::IntoReader(&sink_reader)},
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

Datasets can be used to produce scan nodes, which act like a source.
(It might be useful to think of a scan node as shorthand for constructing
a `Scanner` which scans your dataset, getting a `Reader` from the dataset,
then wrapping that `Reader` into a source node).

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

Datasets may be scanned multiple times, which in this context means one can
produce multiple scan nodes from a single dataset. (Useful
for a self-join, for example.)

???

By contrast: RecordBatchReader is not reusable!

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

In short, `arrow::compute::Kernel`s are mostly *.note[used by]*
ExecNode implementations. For example, `FilterNode` uses potentially
many kernels to evaluate filter conditions and others to materialize
batches with some rows excluded by that filter condition.

???

  (favorite example: one of the addition kernels adds two buffers of
   `int32_t` into a consumer-preallocated buffer of `int32_t`)

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

Dataset scans can cheat this slightly by tagging batches
with file-of-origin info.

`OrderBySinkNode` is the ExecNode which corresponds to
SQL's `ORDER BY`. It sorts the batches *as they leave the graph*.

We may need to change this; some aggregates and window functions
require ordering of their input.

> [Active debate currently being led by Weston Pace.](https://docs.google.com/document/d/1MfVE9td9D4n5y-PTn66kk4-9xG7feXs1zSFf-qxQgPs/edit#)
???

Tagging breaks down some when we split batches for fan out

Our interface for reusability is the stream of batches,
which is unordered.

---

# Eventually...

  - Taskify
  - Backpressure
  - Graceful spill-to-disk/out-of-core execution
  - fan out execution of expressions ala HyPer
  - Maybe allow ordering along some edges of the graph
  - Maybe multiple outputs
  - No more explicit construction. *Everybody* should produce IR
    and have that mapped onto ExecNodes.
  - GPU backed ExecNodes

