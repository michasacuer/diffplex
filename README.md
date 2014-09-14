DiffPlex
========

DiffPlex is C# library to generate textual diffs

In order to use the DiffPlex library you just need to include a reference to DiffPlex.dll in your project.

## About the API
The DiffPlex library currently exposes two interfaces for generating diffs:

* IDiffer (implemented by the Differ class) - This is the core diffing class.  It exposes the low level functions to generate differences between texts.

* ISidebySideDiffer (implemented by the SideBySideDiffer class) - This is a higher level interface.  It consumes the IDiffer interface and generates a SideBySideDiffModel.  This is a model which is suited for displaying the differences of two peices of text in a side by side view.


## Examples
For examples of how to use the API please see the the following projets contained in the DiffPlex solution.

For use of the IDiffer interface see:
* SidebySideDiffer.cs contained in the DiffPlex Project.
* UnidiffFormater.cs contained in the DiffPlex.ConsoleRunner project.

For use of the ISidebySideDiffer interface see:
* DiffController.cs  and associated MVC views in the WebDiffer project
* TextBoxDiffRenderer.cs in the SilverlightDiffer project



## IDiffer Interface 
 
    /// <summary>
    /// Provides methods for generate differences between texts
    /// </summary>
    public interface IDiffer
    {
        /// <summary>
        /// Create a diff by comparing text line by line
        /// </summary>
        /// <param name="oldText">The old text.</param>
        /// <param name="newText">The new text.</param>
        /// <param name="ignoreWhiteSpace">if set to <c>true</c> will ignore white space when determining if lines are the same.</param>
        /// <returns>A DiffResult object which details the differences</returns>
        DiffResult CreateLineDiffs(string oldText, string newText, bool ignoreWhiteSpace);

        /// <summary>
        /// Create a diff by comparing text character by character
        /// </summary>
        /// <param name="oldText">The old text.</param>
        /// <param name="newText">The new text.</param>
        /// <param name="ignoreWhitespace">if set to <c>true</c> will treat all whitespace characters are empty strings.</param>
        /// <returns>A DiffResult object which details the differences</returns>
        DiffResult CreateCharacterDiffs(string oldText, string newText, bool ignoreWhitespace);

        /// <summary>
        /// Create a diff by comparing text word by word
        /// </summary>
        /// <param name="oldText">The old text.</param>
        /// <param name="newText">The new text.</param>
        /// <param name="ignoreWhitespace">if set to <c>true</c> will ignore white space when determining if words are the same.</param>
        /// <param name="separators">The list of characters which define word separators.</param>
        /// <returns>A DiffResult object which details the differences</returns>
        DiffResult CreateWordDiffs(string oldText, string newText, bool ignoreWhitespace, char[] separators);

        /// <summary>
        /// Create a diff by comparing text in chunks determined by the supplied chunker function.
        /// </summary>
        /// <param name="oldText">The old text.</param>
        /// <param name="newText">The new text.</param>
        /// <param name="ignoreWhiteSpace">if set to <c>true</c> will ignore white space when determining if chunks are the same.</param>
        /// <param name="chunker">A function that will break the text into chunks.</param>
        /// <returns>A DiffResult object which details the differences</returns>
        DiffResult CreateCustomDiffs(string oldText, string newText, bool ignoreWhiteSpace, Func<string, string[]> chunker);
    }


## ISidebySideDiffer Interface

    /// <summary>
    /// Provides methods that generate differences between texts for displaying in a side by side view.
    /// </summary>
    public interface ISideBySideDiffBuilder
    {
        /// <summary>
        /// Builds a diff model for  displaying diffs in a side by side view
        /// </summary>
        /// <param name="oldText">The old text.</param>
        /// <param name="newText">The new text.</param>
        /// <returns>The side by side diff model</returns>
        SideBySideDiffModel BuildDiffModel(string oldText, string newText);
    }
