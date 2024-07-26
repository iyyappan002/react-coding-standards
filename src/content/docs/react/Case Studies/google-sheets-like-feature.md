---
title: Google Sheets-like Feature
description: Google Sheets-like Feature.
---

Developing a ReactJS application with a Google Sheets-like feature for creating, editing, and managing spreadsheets. The application needs to provide extensive spreadsheet functionalities.

To implement google sheet like feature we used <a href="https://univer.ai/guides/sheet/getting-started/quickstart" target="_blank">Univer Package</a>

## About Univer Package

Univer provides a comprehensive enterprise-level solution for document and data collaboration, supporting three core document types: spreadsheets, documents, and slides. Through a flexible API and plugin mechanism, developers can customize and extend personalized features on Univer to meet the specific needs of different users in various scenarios.

:::note
To install **univer** package, check out the  <a href="https://univer.ai/guides/sheet/getting-started/installation" target="_blank">Univer Installation Documentation</a>
:::

## Features

1. **Editing Operations**
    -   Create, delete and re-order worksheets
    -   Cell styles: bold, italic, underline, strike-through, font family, font size, text color, background color, border style, text alignment, text rotation
    -   Merged cells: merge and unmerge cells, merge cells in different directions
    -   Row column operations: insert, delete, and move rows/columns, modify row height and column width
    -   Copy & paste: paste values only, paste format only, paste formula only, paste column width only, paste border style only
    -   Clear content: content only, style only, all
    -   Cell editing: cell editor that support rich-format texts
    -   Insert and delete cells
    -   Auto fill
    -   Format painter
    -   Cell comments
    -   Insert floating images
    -   Insert charts
2. **Viewing**
    -   Freeze column and/or rows
    -   Summary bar
        -Sum, Max, Min, Average, Count
3. **Data and Calculation**
    -   Formulas
        -   Fx bar (formula editor)
        -   Formula highlighting
        -   Built-in formulas
        -   Formula calculation using Web Workers (optional)
    -   Number format: general, accounting, currency, date, thousands separator
    -   Data validation
    -   Conditional formatting
    -   Data sorting
    -   Data filtering


## Example

```jsx title="SpreadSheet.jsx"
import { useEffect } from "react";

import "@univerjs/design/lib/index.css";
import "@univerjs/ui/lib/index.css";
import "@univerjs/docs-ui/lib/index.css";
import "@univerjs/sheets-ui/lib/index.css";
import '@univerjs/sheets-filter-ui/lib/index.css';
import "@univerjs/sheets-formula/lib/index.css";

import { LocaleType, Tools, Univer, UniverInstanceType } from "@univerjs/core";
import { defaultTheme } from "@univerjs/design";
import { UniverFormulaEnginePlugin } from "@univerjs/engine-formula";
import { UniverRenderEnginePlugin } from "@univerjs/engine-render";
import { UniverUIPlugin } from "@univerjs/ui";
import { UniverDocsPlugin } from "@univerjs/docs";
import { UniverDocsUIPlugin } from "@univerjs/docs-ui";
import { UniverSheetsPlugin } from "@univerjs/sheets";
import { UniverSheetsFormulaPlugin } from "@univerjs/sheets-formula";
import { UniverSheetsUIPlugin } from "@univerjs/sheets-ui";
import { UniverSheetsFilterPlugin } from '@univerjs/sheets-filter';
import { UniverSheetsFilterUIPlugin } from '@univerjs/sheets-filter-ui';

import DesignEnUS from '@univerjs/design/locale/en-US';
import UIEnUS from '@univerjs/ui/locale/en-US';
import DocsUIEnUS from '@univerjs/docs-ui/locale/en-US';
import SheetsEnUS from '@univerjs/sheets/locale/en-US';
import SheetsUIEnUS from '@univerjs/sheets-ui/locale/en-US';
import SheetsFilterUIEnUS from '@univerjs/sheets-filter-ui/locale/en-US';

import { FUniver } from "@univerjs/facade";
import { useDispatch, useSelector } from "react-redux";
import { getNotes, saveNotes } from "../../store/notes/noteApi";

function SpreadSheet({ savedNotes }) {
    useEffect(() => {
        const univer = new Univer({
            theme: defaultTheme,
            locale: LocaleType.EN_US,
            locales: {
                [LocaleType.EN_US]: Tools.deepMerge(
                    SheetsEnUS,
                    DocsUIEnUS,
                    SheetsUIEnUS,
                    UIEnUS,
                    DesignEnUS,
                    SheetsFilterUIEnUS,
                ),
            },
        });

        univer.registerPlugin(UniverRenderEnginePlugin);
        univer.registerPlugin(UniverFormulaEnginePlugin);

        univer.registerPlugin(UniverUIPlugin, {
            container: 'notessheets',
        });

        univer.registerPlugin(UniverDocsPlugin, {
            hasScroll: false,
        });
        univer.registerPlugin(UniverDocsUIPlugin);

        univer.registerPlugin(UniverSheetsPlugin);
        univer.registerPlugin(UniverSheetsUIPlugin);
        univer.registerPlugin(UniverSheetsFormulaPlugin);
        univer.registerPlugin(UniverSheetsFilterPlugin);
        univer.registerPlugin(UniverSheetsFilterUIPlugin);

        const workbook = univer.createUnit(UniverInstanceType.UNIVER_SHEET, savedNotes ? savedNotes : {});
        const univerAPI = FUniver.newAPI(univer)

        const disposeSelectionListener = univerAPI.onCommandExecuted((command) => {
            const { id } = command;
            console.log({ command });
            if (id === 'sheet.mutation.set-range-values') {
                //your current active saved data
                const savedData = univerAPI.getActiveWorkbook()?.getSnapshot();
            }
        });
        return () => {
            if (disposeSelectionListener) {
                disposeSelectionListener.dispose()
            }
            workbook.dispose()
        }
    }, [])
    return (
        <div id='notessheets' className="h-full w-full"></div>
    )
}

export default SpreadSheet
```

## Conclusion

By leveraging the **univer** package, successfully provided a Google Sheets-like experience and a user-friendly interface.