%!TEX TS-program = ../make.zsh

\newcommand\done{\checkmark\xspace}
\newcommand\inprogress{$\Rightarrow$\xspace}
\newcommand\tobedone{$\square$\xspace}

\newcommand\question[1]{\fbox{\ \ \questionicon \ \ #1}}

\begin{frame}[fragile]{State of the review}

  \begin{itemize}
    \item Working on review since Jun 2019, interrupted by holiday season and urgent service tasks at ECAP.
    \item Currently trying to get the existing tests running.
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
  \end{itemize}

\end{frame}

\begin{frame}[fragile]{First glance}

  \begin{itemize}
    \item The project appears to be well structured.
    \item **Documentation** does exist in the \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/resources/docs}{`resources/docs`} directory.

      The \href{https://wiki.icecube.wisc.edu/index.php/Relativistic_Monopole_Simulation}{wiki contains similar information}, which is not in sync, however, with the \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/resources/docs}{`resources/docs`} directory.

      Also, a `README.md` in the project root is not present.
    \item **Example scripts**: One example script does exist in \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/resources/examples}{`resources/examples`}.
    \item **Tests** do exist: There are python tests in \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/resources/examples}{`resources/test`} and cxx tests in \href{https://github.com/fiedl/monopole-generator/tree/f68f19ff7a4d8fead45b4d6ae345723da69edbf6/private/test}{`private/test`} as expected.

      However, currently, **test do not run**.
  \end{itemize}

\end{frame}

\begin{frame}[fragile]{Trying out the tests}

  \begin{itemize}
    \item Tests fail with `terminate called after throwing an instance of 'boost::python::error_already_set'`
    \item Details: \url{https://github.com/fiedl/monopole-generator/issues/2}
    \item To have this issue reproducible: use github-actions (similar to travis ci, but faster and longer cpu time):
      \begin{itemize}
        \item \githubicon \url{https://github.com/fiedl/icecube-simulation-install}
        \item \githubicon \url{https://github.com/fiedl/monopole-generator-install}
        \item This tests the build of icecube-simulation and the monopole-generator in a build matrix against the latest stable release of icecube-simulation and the current `trunk`, for ubuntu and macOS.
      \end{itemize}
  \end{itemize}

\end{frame}

\begin{frame}[fragile]{Project source code}

  \begin{itemize}
    \item The `monopole-generator` source code can be found on the icecube svn: \url{http://code.icecube.wisc.edu/svn/projects/monopole-generator/}
    \item Also, the code has been mirrored to github: \\ \githubicon \url{https://github.com/fiedl/monopole-generator}
    \item Currently, the code **does not contain a license**.

      \question{Can the github repo be made public?}
  \end{itemize}

\end{frame}


