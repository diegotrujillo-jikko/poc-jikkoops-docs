

Hello, I'm Nick. In this video, I will
share best practices for creating CLOM
MD file for CL code.
But first, let's discuss what CloudMD
is. Clom MD is a file for providing
project specific context to CL code.
Whenever CLA AI model writes or modifies
existing code in your project, the first
thing it will do is check the clo MD
file. The file is typically placed in
the root folder of your project. For
example, if you're working with React
web app, you should have this structure
and the file itself is structured in
markdown format. MD stands for markdown.
The good news is that you don't need to
create clo MD from scratch. When you are
in directory of your project, you can
run init command.
Clo will scan the entire project
codebase and generate MD file for you.
It will include all essential details
such as project architecture conventions
in this file. So you don't have to
provide these things manually. But you
should always treat the cloth MD file
that clo code generate for you only as a
first draft. There is a high chance the
file will miss important sections. So
it's a good idea to refine it based on
the following guide. And now let's dive
into the best practices. Every project
is different and it's impossible to
provide one fits all structure for CloMD
file. But it's still possible to name 10
key sections that will be relevant to
most projects. And the first section is
project overview. This is the highest
value section that creates context for
code. In this section, you should
explain in plain language what the
project is, who is it for, and the most
important business or ex constraints.
Try to keep this to a few paragraphs.

And here is an example for your
reference.
Next comes text tag. This section
prevents a load of bad assumptions.
Without this section, CLO may introduce
libraries that are technically valid but
wrong for your project. State the actual
technologies in use such as frameworks,
programming languages, styling systems,
and so on. Also include what not to use.
Next is architecture.
This is where you teach claude how the
repository is organized.
You need to describe major directories,
responsibilities of each area, data
flow, where new code should go. Next is
coding conventions along with project
overview. I think this is the second
most important section in the whole file
because it has direct impact on the
quality of the code output produced by
code and you need to include anything
that impacts day-to-day code generation
in this section.
Moving next UI and design system rules
for front- end projects like websites.
This section is pure gold. You can
define here visual styles, spacing
philosophy, typography approach,
interaction patterns, responsiveness,
and accessibility expectations.
Moving next, content and copy guidance.
This section is vital especially for
langing pages and product work because
content is king. State how copy should
sound, concise or detailed, technical or
plain language, aspirational or
practical.
Section number seven, testing and
quality bar.
CL should know how finished work is
validated. Define what tests to add,
when tests are required,
and what done actually means for CL.

Moving next, file and content placement
rules. This section is especially useful
in mature repositories because duplicate
components become a problem fast. You
need to define rules for when to create
new file, when to edit existing files,
when to create abstractions, as well as
naming patterns. Moving next, save
change roles. Very valuable for real
projects. It's a good idea to tell CL
what it should avoid changing casually.
It will reduce technically smart but
operationally risky edits that will lead
to expensive refactorings. Finally,
specific commands. Entropic recommends
giving Clot a concrete project context
and commands are part of the operational
context. Add the actual commands CLO
should use and of course only include
commands that are real and current.
And here is a quick preview of how CLMD
will look like for a web project landing
page crafted with the React framework.
As you can see, this file was built on
top of the original draft that Claude
created for us, but it's way more
detailed and sophisticated. Finally, as
a rule of thumb, I suggest keeping the
CLOTM MD file under 200 lines because it
will make the file scannable and easier
for humans to read. And that's all. Let
me know what you think about the
structure of CLMD file that I shared
with you in the comments. Thank you.

