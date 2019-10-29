%!TEX TS-program = ../make.zsh

\newcommand\done{\checkmark\xspace}
\newcommand\inprogress{$\Rightarrow$\xspace}
\newcommand\tobedone{$\square$\xspace}

\newcommand\question[1]{\colorbox{blue!10}{\fbox{\ \ \questionicon \ \ #1}}}

\begin{frame}[fragile]{Overview of the review}

  \begin{itemize}
    \item State of the review:
      \begin{itemize}
        \item[\done] First glance: Documentation, example scripts, unit tests, build
        \item[\done] Directory structure
        \item[\inprogress] Build, run, and review tests
        \item[\inprogress] Review code structure and readability
        \item[\tobedone] Review documentation
        \item[\tobedone] Review coding standards
        \item[\tobedone] Review usability
      \end{itemize}

      \ \ \ \tiny See also: \url{http://software.icecube.wisc.edu/documentation/general/code_reviews.html} \normalsize

    \item Current state documented in \githubicon \url{https://github.com/fiedl/monopole-generator/issues/1}.

      Results will be published to \href{https://github.com/fiedl/monopole-generator/tree/master/resources/docs}{`resources/docs`} after completing the review.

    \item This talk: **Tests**
  \end{itemize}

\end{frame}

\begin{frame}[fragile]{How to run the tests}
  \begin{itemize}

    \item There are **python tests** in \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/resources/examples}{`resources/test`} and **cxx tests** in \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/private/test}{`private/test`} as expected.

    \item How to run the **python tests**:
      \begin{bash}
        [2019-10-29 23:16:56] fiedl@fiedl-mbp ~/icecube/software/icecube-combo-stable-2019-10-29/debug_build
        ▶ ./env-shell.sh python ../src/monopole-generator/resources/test/test_monopole_generator.py
        ▶ ./env-shell.sh python ../src/monopole-generator/resources/test/test_monopole_propagator.py
      \end{bash}

    \item How to run the **cxx tests**:
      \begin{bash}
        [2019-10-29 23:16:56] fiedl@fiedl-mbp ~/icecube/software/icecube-combo-stable-2019-10-29/debug_build
        ▶ make test-bins
        ▶ ./env-shell.sh bin/monopole-generator-test --all
      \end{bash}

    \item There is also a `make monopole-generator-test` in the `monopole-generator/Makefile`.

      \begin{bash}
        [2019-10-30 00:08:42] fiedl@fiedl-mbp ~/icecube/software/icecube-combo-stable-2019-10-29/debug_build/monopole-generator
        ▶ ../env-shell.sh make monopole-generator-test
      \end{bash}

      \question{Is this the same as in `make test-bins` from the root directory?}

  \end{itemize}
\end{frame}

\begin{frame}[fragile]{Running the tests \& test results}
  \begin{itemize}

    \item **Last time**: Tests did not run due to a **missing `./env-shell.sh`** \\
      \url{https://github.com/fiedl/monopole-generator/issues/2}

    \item Now: **Python tests** do run and **succeed**.

      \begin{bash}
        [2019-10-29 21:21:04] fiedl@fiedl-mbp ~/icecube/software/icecube-simulation-V06-01-01
        ▶ debug_build/env-shell.sh python src/monopole-generator/resources/test/test_monopole_generator.py
        ----------------------------------------------------------------------
        Ran 40 tests in 159.557s
        OK

        [2019-10-29 21:24:00] fiedl@fiedl-mbp ~/icecube/software/icecube-simulation-V06-01-01
        ▶ debug_build/env-shell.sh python src/monopole-generator/resources/test/test_monopole_propagator.py
        ----------------------------------------------------------------------
        Ran 4 tests in 0.014s
        OK
      \end{bash}

    \item **Cxx tests** do **run**, but **some fail**.

      \begin{bash}
        [2019-10-29 21:18:46] fiedl@fiedl-mbp ~/icecube/software/icecube-simulation-V06-01-01/debug_build
        ▶ ./env-shell.sh bin/monopole-generator-test --all
        ===================================================================
        Pass: 10
        Fail: 4
         THESE TESTS FAIL
            SlowMonopoleTest.cxx/DefaultSecondariesTest
            SlowMonopoleTest.cxx/GeneralSecondariesTest
            SlowMonopoleTest.cxx/TestMFP
            SlowMonopoleTest.cxx/TestScaling
      \end{bash}

  \end{itemize}
\end{frame}

\begin{frame}[fragile]{Why do tests fail?}
  \begin{itemize}
    \item Failure message:

      \begin{bash}
        FATAL (I3MonopoleGenerator): Requested less than 1 event to be generated. Abort! (I3MonopoleGenerator.cxx:92 in virtual void I3MonopoleGenerator::Configure())
      \end{bash}

    \item There are consistency checks in the code:

      \begin{cpp}
        // src/monopole-generator/private/monopole-generator/I3MonopoleGenerator.cxx
        GetParameter("NEvents", Nevents_);
        GetParameter("TreeName", treeName_);
        GetParameter("Mass", mass_);
        // ...
        if (Nevents_<= 0){
          log_fatal("Requested less than 1 event to be generated. Abort!");
        }
      \end{cpp}

    \item Maybe the consistency checks were not present when the tests were created.
    \item Not sure why the CI did not complain when introducing the consistency checks.

  \end{itemize}
\end{frame}

\begin{frame}[fragile]{Fixing the tests}
  \begin{itemize}

    \item Setting `NEvents` in the test fixes the issue, but then the test runs in the next consistency check.
    \item[\inprogress] Need to fix the tests by adding the minimum of required parameters.

    \item \question{Should I fix the tests?}
    \item \question{When should the tests be fixed? During the review or afterwards?}
    \item \question{Where should the tests be fixed? What's our workflow?}

      \small I would fix them in a branch on \githubicon \url{https://github.com/fiedl/monopole-generator}. Should I mirror the fixes to a feature branch on the svn? What's our svn workflow?
  \end{itemize}
\end{frame}

\begin{frame}[fragile]{Testing against combo}
  \begin{itemize}

    \item On Monday, I've learned that \href{http://code.icecube.wisc.edu/svn/meta-projects/simulation}{icecube-simulation} is discontinued and everyone should switch to \href{http://code.icecube.wisc.edu/svn/meta-projects/combo}{icecube-combo}.

    \item \question{What is Combo? Where can I find documentation?}

  \end{itemize}
\end{frame}