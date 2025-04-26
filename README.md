# cs316-lab-7-solved
**TO GET THIS SOLUTION VISIT:** [CS316 Lab 7 Solved](https://www.ankitcodinghub.com/product/cs316-compilers-lab-solved-12/)


---

📩 **If you need this solution or have special requests:** **Email:** ankitcoding@gmail.com  
📱 **WhatsApp:** +1 419 877 7882  
📄 **Get a quote instantly using this form:** [Ask Homework Questions](https://www.ankitcodinghub.com/services/ask-homework-questions/)

*We deliver fast, professional, and affordable academic help.*

---

<h2>Description</h2>



<div class="kk-star-ratings kksr-auto kksr-align-center kksr-valign-top" data-payload="{&quot;align&quot;:&quot;center&quot;,&quot;id&quot;:&quot;126901&quot;,&quot;slug&quot;:&quot;default&quot;,&quot;valign&quot;:&quot;top&quot;,&quot;ignore&quot;:&quot;&quot;,&quot;reference&quot;:&quot;auto&quot;,&quot;class&quot;:&quot;&quot;,&quot;count&quot;:&quot;1&quot;,&quot;legendonly&quot;:&quot;&quot;,&quot;readonly&quot;:&quot;&quot;,&quot;score&quot;:&quot;5&quot;,&quot;starsonly&quot;:&quot;&quot;,&quot;best&quot;:&quot;5&quot;,&quot;gap&quot;:&quot;4&quot;,&quot;greet&quot;:&quot;Rate this product&quot;,&quot;legend&quot;:&quot;5\/5 - (1 vote)&quot;,&quot;size&quot;:&quot;24&quot;,&quot;title&quot;:&quot;CS316 Lab 7 Solved&quot;,&quot;width&quot;:&quot;138&quot;,&quot;_legend&quot;:&quot;{score}\/{best} - ({count} {votes})&quot;,&quot;font_factor&quot;:&quot;1.25&quot;}">

<div class="kksr-stars">

<div class="kksr-stars-inactive">
            <div class="kksr-star" data-star="1" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" data-star="2" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" data-star="3" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" data-star="4" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" data-star="5" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
    </div>

<div class="kksr-stars-active" style="width: 138px;">
            <div class="kksr-star" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
            <div class="kksr-star" style="padding-right: 4px">


<div class="kksr-icon" style="width: 24px; height: 24px;"></div>
        </div>
    </div>
</div>


<div class="kksr-legend" style="font-size: 19.2px;">
            5/5 - (1 vote)    </div>
    </div>
1 Introduction

2 Liveness

The first step in performing register allocation is performing a liveness analysis. We are asking you to perform liveness analysis across an entire function at once. See Week 12 (Slide 6 onwards) for more details.

2.1 Control flow graphs

The first step in computing liveness is to build a control flow graph for each function in your program. To represent your control flow graph, each IR Node should know its successors (IR instructions that could possibly execute immediately after it) and predecessors (IR instructions that could possible execute immediately before it). Conditional jumps have two successors: the explicit target of the jump, and the implicit (fall-through) target of the jump. Unconditional jumps only have one successor. Function calls should be treated as straight-line IR nodes (i.e., they are not treated as branches; their successor is the instruction immediately after the call). Return nodes do not have any successors.

2.2 Computing Liveness

For each IR node in a function, you should define two sets: GEN and KILL. GEN represents all the temporaries and variables that are used in an instruction, and KILL represents all the temporaries and variables that are defined in an instruction. For most instructions, this should be pretty straightforward. A few tricky cases:

• PUSH instructions use the variable/temporary being pushed • POP instructions define the variable/temporary being popped

• WRITE instructions use their variables.

• READ instructions define their variables.

• The set of variables that are live out of a node is the union of all the variables that are live in to the node’s successors.

• The set of variables that are live in to a node is the set of variables that are live out for the node, minus any variables that are killed by the node, plus any variables that are gen-ed by the node.

Note that in these definitions are recursive: the live-out set of a node is defined in terms of the live-in sets of its successors, which are in turn defined in terms of the live-in sets of their successors, and so on. If there is a loop in the code, then the definition seems circular.

The trick to computing liveness is to compute a fixpoint: assignments to each of the live-in and live-out sets so that if you try to compute any node’s live-in or live-out set again, you’ll get the same result you already have. To do this, we will use a worklist algorithm:

1. Put all of the IR nodes on the worklist

2. Pull an IR node off the worklist, and compute its live-out and live-in sets according to the definitions above.

4. Repeat steps 2 and 3 until the worklist is empty.

(Note: you can write a slower version of this code that ignores identifying nodes’ predecessors. This still works. However, we suggest you to put all the IR nodes on the worklist and process all of them. If any live-in or live-out set has changed, put all the IR nodes on the work list and repeat the process.)

3 Register Allocation Algorithm

Use the bottom-up register allocation algorithm or the graph-coloring method discussed in class. For each statement, you must ensure that the source operands are in registers, and that there is a register for the destination operand. Use the liveness information you computed (i.e., the live-out set for the instruction) to determine when it is safe to free registers, and when a dirty register needs to be stored back to memory (only when the variable in the register is live).

Bottom-up register allocation works at the basic-block level: any register allocation decisions you make apply for the current basic block only. This means that when you get to the end of a basic block, you must reset your register allocation. Any register that (a) holds local/global variables and (b) is dirty should be written back to the stack/global variable.

Note also that because a CALL instruction jumps into another method, any global variables that are in registers when the CALL is performed should be freed immediately prior to the CALL instruction, ensuring that the correct value for the global is in memory. This is different from saving the registers on the stack prior to a function call. The latter is done so that the caller method doesn’t get its registers overwritten; the values of the registers are stored where only the caller can see them. The former is done so that the callee method sees the right values for global variables; the values need to be stored back to globals so that everyone can see them, and freed from the registers so that the caller will reload them after the callee returns.

Testing your Tiny code You can test your Tiny code by using, tiny4regs.C, a version of the simulator that limits you to 4 registers. tiny4regs.C is provided to you along with the starter files. There are no test cases provided along with the starter files for this assignment. You must test your compiler with all the test cases of PA4, PA5, and PA6. In addition, we will test with some hidden test cases.

4 What you need to do

Perform the liveness analysis and register allocation steps as described above, so that your compiler generates code that only uses 4 registers.

Handling errors All the inputs we will give you in this step will be valid programs. We will also ensure that all expressions are type safe: a given expression will operate on either INTs or FLOATs, but not a mix, and all assignment statements will assign INT results to variables that are declared as INTs (and respectively for FLOATs).

Grading In this step, we will only grade your compiler on the correctness of the generated code. We will run your generated code through the Tiny simulator and check to make sure that you produce the same result as our code. When we say result, we mean the outputs of any WRITE statements in the program (not details such as how many cycles the code uses, how many registers, etc.)

5 What you need to submit

• Place all the necessary code for your compiler that you wrote yourself.

• A Makefile with the following targets:

1. compiler: this target will build your compiler. (-1 for warnings)

2. clean: this target will remove any intermediate files that were created to build the compiler. (-1 for not doing the clean properly)

3. dev: this target will print the same information that you printed in previous PA.

• A shell script (this must be written in bash) called runme that runs your compiler. This script should take in two arguments: first, the input program file to be compiled and second, the filename where you want to put the compiler. You can assume that we will have run make compiler before running this script.

• You should tag your programming assignment submission as cs316pa7submission

Do not submit any binaries. Your git repo should only contain source files; no products of compilation. If you have a folder named test in your repo, it will be deleted as part of running our test script (though the deletion won’t get pushed) – make sure no code necessary for building/running your compiler is in such a directory.
